# Add Calendar Event for Client Management

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — permission to modify the contact
>
> - [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) — a user with permission to read contacts
> - [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) — a user with permission to modify the CRM object the activity is added to

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Calendar events can be added automatically to remind employees about meetings or calls with clients. A "Meeting" activity appears in the contact card, and Bitrix24 mirrors it as an event in the personal calendar of the responsible employee: the event title is taken from `SUBJECT`, and its time frame — from `START_TIME` and `END_TIME`.

The key parameters of the scenario are `OWNER_TYPE_ID` and `TYPE_ID`. `OWNER_TYPE_ID` defines which CRM object's card the activity appears in, and `TYPE_ID` defines what kind of activity it is. Only a meeting creates an event in the calendar, so we pass `1` in `TYPE_ID`.

The method that creates the activity does not retrieve the client data on its own: the phone number for `COMMUNICATIONS` and the responsible person for `RESPONSIBLE_ID` have to be retrieved from the contact card first. That is why the scenario consists of two steps.

1. Retrieve the phone number and the responsible person using the [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) method

2. Create the activity using the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method, substituting the retrieved values into `COMMUNICATIONS` and `RESPONSIBLE_ID`

As a result, the method returns the activity ID, the activity appears in the contact timeline, and the event appears in the calendar of the responsible person.

## Before You Start

- The contact is already created in Bitrix24, and you know its ID. The ID is returned by the [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) and [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) methods

- The contact has a phone number filled in. Without a communication, the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method does not create the activity and returns the `The field COMMUNICATIONS is not defined or invalid` error

- The contact has the "Responsible person" field filled in. Its identifier goes into `RESPONSIBLE_ID`, and the event appears in the calendar of exactly that employee

- The webhook is created on behalf of a user who can modify this contact. The method checks permissions not on the activity, but on the CRM object the activity is attached to

## 1. Retrieve Client Data

We will use the [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) method with the client ID. Replace `1` with the ID of your own contact.

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const contactID = 1;
    const response = await $b24.actions.v2.call.make({
        method: 'crm.contact.get',
        params: { id: contactID },
        requestId: 'contact-get'
    })
    const resultContact = response.getData().result;
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client


    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    contact_id = 1
    result_contact = client.crm.contact.get(
        bitrix_id=contact_id,
    ).response.result
    ```

- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $contactID = 1;
    $resultContact = $sb->getCRMScope()->contact()->get($contactID)->contact();
    ```

- Go

    ```go
    // core, ctx, and contactID are declared in the full example below
    res, err := core.Call(ctx, "crm.contact.get",
    	b24.Params{"id": contactID}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.contact.get: %w", err)
    }

    // The phone and the responsible person are needed from the response. PHONE is a multifield: a list of
    // objects, even when there is a single number, and it arrives only if the contact
    // has any phone numbers at all.
    var contact struct {
    	ID           b24.ID `json:"ID"`
    	Name         string `json:"NAME"`
    	LastName     string `json:"LAST_NAME"`
    	AssignedByID b24.ID `json:"ASSIGNED_BY_ID"`
    	Phone        []struct {
    		ID        b24.ID `json:"ID"`
    		Value     string `json:"VALUE"`
    		ValueType string `json:"VALUE_TYPE"`
    	} `json:"PHONE"`
    }
    if err := json.Unmarshal(res.Result, &contact); err != nil {
    	return fmt.Errorf("parse contact: %w", err)
    }
    if len(contact.Phone) == 0 {
    	return fmt.Errorf("contact %d has no phone number", contactID)
    }
    ```

{% endlist %}

As a result, we will obtain the client data. Save two values for the next step:

- `PHONE[0].VALUE` — the phone number. This is a multifield: the method returns a list of objects even when there is a single number, and it does not return the `PHONE` key at all if the contact has no phone numbers

- `ASSIGNED_BY_ID` — the identifier of the responsible employee

The rest of the response fields are not needed for the scenario.

```json
{
    "result": {
        "ID": "1",
        "POST": "Managing Director",
        "COMMENTS": null,
        "NAME": "Klaus",
        "SECOND_NAME": "Werner",
        "LAST_NAME": "Müller",
        "PHOTO": null,
        "LEAD_ID": null,
        "TYPE_ID": "SHARE",
        "SOURCE_ID": "SELF",
        "SOURCE_DESCRIPTION": null,
        "COMPANY_ID": "52",
        "BIRTHDATE": "",
        "EXPORT": "Y",
        "HAS_PHONE": "Y",
        "HAS_EMAIL": "Y",
        "HAS_IMOL": "N",
        "DATE_CREATE": "2023-08-18T12:43:42+03:00",
        "DATE_MODIFY": "2023-10-17T15:59:13+03:00",
        "ASSIGNED_BY_ID": "61",
        "CREATED_BY_ID": "57",
        "MODIFY_BY_ID": "47",
        "OPENED": "N",
        "ORIGINATOR_ID": null,
        "ORIGIN_ID": null,
        "ORIGIN_VERSION": null,
        "FACE_ID": null,
        "LAST_ACTIVITY_TIME": "2025-03-15T10:38:21+02:00",
        "ADDRESS": null,
        "ADDRESS_2": null,
        "ADDRESS_CITY": null,
        "ADDRESS_POSTAL_CODE": null,
        "ADDRESS_REGION": null,
        "ADDRESS_PROVINCE": null,
        "ADDRESS_COUNTRY": null,
        "ADDRESS_LOC_ADDR_ID": null,
        "UTM_SOURCE": null,
        "UTM_MEDIUM": null,
        "UTM_CAMPAIGN": null,
        "UTM_CONTENT": null,
        "UTM_TERM": null,
        "LAST_ACTIVITY_BY": "1",
        "PHONE": [
            {
                "ID": "1326",
                "VALUE_TYPE": "MOBILE",
                "VALUE": "498001001020",
                "TYPE_ID": "PHONE"
            }
        ],
        "EMAIL": [
            {
                "ID": "1328",
                "VALUE_TYPE": "WORK",
                "VALUE": "mueller@example.com",
                "TYPE_ID": "EMAIL"
            }
        ]
    },
    "time": {
        "start": 1747737934.888428,
        "finish": 1747737934.945823,
        "duration": 0.057394981384277344,
        "processing": 0.029510021209716797,
        "date_start": "2025-05-20T13:45:34+03:00",
        "date_finish": "2025-05-20T13:45:34+03:00"
    }
}
```

## 2. Create Calendar Event

To create the activity and the calendar event, we will use the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method with the following parameters:

- `SUBJECT` — the activity title, which also becomes the event title in the calendar. We will specify `calendar title`. The method does not accept an empty string

- `DESCRIPTION` — description. For example, `calendar body`

- `DESCRIPTION_TYPE` — format of the description text: `1` — plain text, `2` — HTML markup, `3` — BB code. We will set the value to `3`

- `OWNER_ID` — the identifier of the CRM object in whose card the activity appears. We pass `contactID` — the contact identifier from step 1

- `OWNER_TYPE_ID` — [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). We pass `3` — contact. A full list of object types is returned by the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method

- `TYPE_ID` — the activity type. We will specify `1` — meeting. The method accepts only `1` — meeting, `2` — call, `4` — e-mail, and `6` — an activity of an external provider. For any other value it returns an error

- `COMMUNICATIONS` — client contact details. Exactly one communication is allowed for a meeting:

    - `VALUE` — the phone number, we take `PHONE[0].VALUE` from the response of step 1

    - `ENTITY_ID` — the client identifier, we pass `contactID`

    - `ENTITY_TYPE_ID` — [object type identifier](../../../api-reference/crm/data-types.md#object_type), we pass `3` — contact

We do not pass the `TYPE` key in `COMMUNICATIONS` for a meeting: Bitrix24 fills it in automatically for calls and e-mails only, and for a meeting it stays empty.

- `START_TIME` and `END_TIME` — start and end date and time in [ISO 8601](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) format. The same values become the boundaries of the calendar event, we will specify a duration of one hour. Replace the dates from the example with future ones: the method does create a meeting in the past, but there will be no reminder for it

- `RESPONSIBLE_ID` — the identifier of the responsible person, we pass `ASSIGNED_BY_ID` from the response of step 1. The event appears in the personal calendar of exactly that employee

We do not pass the `COMPLETED` field in this scenario: the meeting is scheduled, not completed.

{% list tabs %}

- JS

    ```javascript
    const contactPhone = resultContact.PHONE[0];

    const response = await $b24.actions.v2.call.make({
        method: 'crm.activity.add',
        params: {
            fields: {
                "SUBJECT": "calendar title",
                "DESCRIPTION": "calendar body",
                "DESCRIPTION_TYPE": 3,
                "OWNER_ID": contactID,
                "OWNER_TYPE_ID": 3,
                "TYPE_ID": 1,
                "COMMUNICATIONS": [
                    {
                        'VALUE': contactPhone.VALUE,
                        'ENTITY_ID': contactID,
                        'ENTITY_TYPE_ID': 3
                    }
                ],
                "START_TIME": "2025-05-20T14:00:00",
                "END_TIME": "2025-05-20T15:00:00",
                "RESPONSIBLE_ID": resultContact.ASSIGNED_BY_ID
            }
        },
        requestId: 'activity-add'
    });
    ```

- Python

    ```python
    contact_phone = result_contact["PHONE"][0]

    response = client.crm.activity.add(
        fields={
            "SUBJECT": "calendar title",
            "DESCRIPTION": "calendar body",
            "DESCRIPTION_TYPE": 3,
            "OWNER_ID": contact_id,
            "OWNER_TYPE_ID": 3,
            "TYPE_ID": 1,
            "COMMUNICATIONS": [
                {
                    "VALUE": contact_phone["VALUE"],
                    "ENTITY_ID": contact_id,
                    "ENTITY_TYPE_ID": 3,
                }
            ],
            "START_TIME": "2025-05-20T14:00:00",
            "END_TIME": "2025-05-20T15:00:00",
            "RESPONSIBLE_ID": result_contact["ASSIGNED_BY_ID"],
        }
    ).response
    ```

- PHP

    ```php
    $phones = $resultContact->PHONE;
    $contactPhone = reset($phones);

    $result = $sb->getCRMScope()->activity()->add(
        [
            "SUBJECT" => "calendar title",
            "DESCRIPTION" => "calendar body",
            "DESCRIPTION_TYPE" => 3,
            "OWNER_ID" => $contactID,
            "OWNER_TYPE_ID" => 3,
            "TYPE_ID" => 1,
            "COMMUNICATIONS" => [
                [
                    'VALUE' => $contactPhone->VALUE,
                    'ENTITY_ID' => $contactID,
                    'ENTITY_TYPE_ID' => 3
                ]
            ],
            "START_TIME" => "2025-05-20T14:00:00",
            "END_TIME" => "2025-05-20T15:00:00",
            "RESPONSIBLE_ID" => $resultContact->ASSIGNED_BY_ID,
        ]
    );
    ```

- Go

    ```go
    // core, ctx, and contact are declared in the full example below.
    // The start and end times are in ISO 8601 format. Here it is a one-hour meeting,
    // tomorrow at the same time.
    start := time.Now().Add(24 * time.Hour)

    res, err = core.Call(ctx, "crm.activity.add", b24.Params{
    	"fields": b24.Params{
    		"SUBJECT":     "calendar title",
    		"DESCRIPTION": "calendar body",
    		// 1 — plain text, 2 — HTML, 3 — BB code.
    		"DESCRIPTION_TYPE": 3,
    		"OWNER_ID":         contact.ID,
    		"OWNER_TYPE_ID":    entityTypeContact,
    		// 1 — a meeting; crm.enum.activitytype returns the full list of activity types.
    		"TYPE_ID": 1,
    		// COMMUNICATIONS links the activity to the client's contact details:
    		// the value is taken from the PHONE multifield retrieved in step 1.
    		"COMMUNICATIONS": []b24.Params{{
    			"VALUE":          contact.Phone[0].Value,
    			"ENTITY_ID":      contact.ID,
    			"ENTITY_TYPE_ID": entityTypeContact,
    		}},
    		"START_TIME":     start.Format(time.RFC3339),
    		"END_TIME":       start.Add(time.Hour).Format(time.RFC3339),
    		"RESPONSIBLE_ID": contact.AssignedByID,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.add: %w", err)
    }

    // There is no wrapper: result is the ID of the created activity itself.
    var activityID b24.ID
    if err := json.Unmarshal(res.Result, &activityID); err != nil {
    	return fmt.Errorf("parse event ID: %w", err)
    }
    ```

{% endlist %}

We have created the activity and received its identifier `6915` in response. There is no wrapper in the response: `result` is the number itself. The identifier can be used in the methods for [updating](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md) and [deleting](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-delete.md) an activity.

```json
{
    "result": 6915
}
```

## Verify the Result

Open the contact card in Bitrix24. The meeting is displayed in the card timeline. Open the calendar of the employee from `RESPONSIBLE_ID` — the event with the title from `SUBJECT` is scheduled for the date from `START_TIME`.

Through REST, the contact activities are returned by the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method with the same `OWNER_TYPE_ID` and `OWNER_ID` values as in step 2. The `COMMUNICATIONS` field is returned only when it is specified in `select`.

{% list tabs %}

- JS

    ```javascript
    const checkResponse = await $b24.actions.v2.call.make({
        method: 'crm.activity.list',
        params: {
            filter: {
                "OWNER_TYPE_ID": 3,
                "OWNER_ID": contactID
            },
            select: ['*', 'COMMUNICATIONS'],
            order: { ID: 'DESC' }
        },
        requestId: 'activity-list'
    });

    console.dir(checkResponse.getData().result);
    ```

- Python

    ```python
    activities = client.crm.activity.list(
        filter={
            "OWNER_TYPE_ID": 3,
            "OWNER_ID": contact_id,
        },
        select=["*", "COMMUNICATIONS"],
        order={"ID": "DESC"},
    ).response.result
    ```


- PHP

    ```php
    // crm.activity.list has no wrapper in the SDK — we call the method directly
    $activities = $sb->core->call(
        'crm.activity.list',
        [
            'filter' => [
                'OWNER_TYPE_ID' => 3,
                'OWNER_ID' => $contactID,
            ],
            'select' => ['*', 'COMMUNICATIONS'],
            'order' => ['ID' => 'DESC'],
        ]
    )->getResponseData()->getResult();
    ```
{% endlist %}

The scenario is complete if the response contains an object with the `ID` from step 2, its `TYPE_ID` equals `1`, and `COMMUNICATIONS` holds the client's phone number.

```json
{
    "result": [
        {
            "ID": "6915",
            "OWNER_ID": "1",
            "OWNER_TYPE_ID": "3",
            "TYPE_ID": "1",
            "SUBJECT": "calendar title",
            "START_TIME": "2025-05-20T14:00:00+03:00",
            "END_TIME": "2025-05-20T15:00:00+03:00",
            "COMPLETED": "N",
            "RESPONSIBLE_ID": "61",
            "DESCRIPTION": "calendar body",
            "DESCRIPTION_TYPE": "3",
            "COMMUNICATIONS": [
                {
                    "ID": "1204",
                    "TYPE": "",
                    "VALUE": "498001001020",
                    "ENTITY_ID": "1",
                    "ENTITY_TYPE_ID": "3"
                }
            ]
        }
    ],
    "total": 1
}
```

In the response, numeric fields arrive as strings — `"TYPE_ID": "1"`, even though a number was passed in the request, and the communication `TYPE` is empty for a meeting. These are not signs of an error.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `Access denied.` | The user does not have permission to modify the contact from `OWNER_ID`. Check which user the webhook was created on behalf of ||
|| `Could not find 'CONTACT' with ID: 1` | There is no contact with such `OWNER_ID` in Bitrix24. Take an existing identifier using the [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) method ||
|| `The field SUBJECT is not defined or empty` | An empty string was passed in `SUBJECT`, or the field was omitted ||
|| `The field COMMUNICATIONS is not defined or invalid` | The communication was not passed or was discarded. This happens when the contact has no phone number and an empty value ends up in `VALUE`. Check `PHONE` in the response of step 1 ||
|| `The field RESPONSIBLE_ID is not defined or invalid` | The method could not determine the responsible person: `RESPONSIBLE_ID` holds an empty or non-numeric value, and the object from `OWNER_ID` has no responsible person either. Check `ASSIGNED_BY_ID` in the response of step 1 ||
|| `The field TYPE_ID is not defined or invalid` | A value outside the `1`–`6` range was passed in `TYPE_ID` ||
|| `The activity type "..." is not supported in current context` | A type that is not available through REST was passed in `TYPE_ID`. The method creates only meetings `1`, calls `2`, e-mails `4`, and provider activities `6` ||
|| `The only one communication is allowed for activity of specified type` | More than one communication was passed in a meeting. Leave a single element in `COMMUNICATIONS` ||
|| `Could not build binding. Please ensure that owner info and communications are defined correctly` | Neither `OWNER_ID` with `OWNER_TYPE_ID` nor a communication with `ENTITY_ID` and `ENTITY_TYPE_ID` was passed. The activity has nothing to attach to ||
|#

Repeat the scenario from the step that returned the error. Step 1 does not create anything, so it can be executed any number of times. If step 2 returned the error, the activity was not created: fix the `fields` and repeat only that step.

A separate case is when the method returns an identifier, the activity is present in the contact card, but there is no event in the calendar. Check three conditions:

- a meeting `1` was passed in `TYPE_ID`

- you are looking at the calendar of the employee from `RESPONSIBLE_ID`, not your own

- if the request contained `COMPLETED` with the value `Y`, check the CRM settings. By default, completed meetings are retained in the calendar, but when the setting is disabled, no event is created for them

## Key Considerations

- An activity of any other type does not create a calendar event. A call `2` and an e-mail `4` appear only in the contact timeline

- The event is created in the personal calendar of the employee from `RESPONSIBLE_ID`, not of the request author. If the field is omitted, the method substitutes the person responsible for the CRM object from `OWNER_ID`

- The `DIRECTION` field is not used for meetings. Direction is meaningful only for calls and e-mails

- Running the example again creates one more activity and one more event, duplicates are not filtered out

- The [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method is no longer being developed, but there is no replacement for this scenario: it is the only method that creates a "Meeting" activity with calendar synchronization. The [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) method creates an activity of a different kind — a scheduled to-do without the "Meeting" type

## Code Example

The example combines both steps: it reads the contact, takes the phone number and the responsible person from the response, and creates a "Meeting" activity in the contact card and an event lasting one hour in the employee's calendar. Replace `contactID` with the identifier of your own contact, and `SUBJECT` and `DESCRIPTION` — with your own text.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    async function createCalendarActivity() {
        try {
            var contactID = 1;
            const responseContact = await $b24.actions.v2.call.make({
                method: 'crm.contact.get',
                params: { id: contactID },
                requestId: 'contact-get'
            });
            var resultContact = responseContact.getData().result;

            if (resultContact.ASSIGNED_BY_ID && resultContact.PHONE) {
                var contactPhone = resultContact.PHONE[0];
                var staffID = resultContact.ASSIGNED_BY_ID;
                await $b24.actions.v2.call.make({
                    method: 'crm.activity.add',
                    params: {
                        fields: {
                            "SUBJECT": "calendar title",
                            "DESCRIPTION": "calendar body",
                            "DESCRIPTION_TYPE": 3, // text type (crm.enum.contenttype): plain, HTML, BB-code
                            "OWNER_ID": contactID,
                            "OWNER_TYPE_ID": 3, // crm.enum.ownertype
                            "TYPE_ID": 1, // crm.enum.activitytype
                            "COMMUNICATIONS": [
                                {
                                    'VALUE': contactPhone.VALUE,
                                    'ENTITY_ID': contactID,
                                    'ENTITY_TYPE_ID': 3 // crm.enum.ownertype
                                }
                            ],
                            "START_TIME": new Date().toISOString(),
                            "END_TIME": new Date(new Date().getTime() + 3600 * 1000).toISOString(),
                            "RESPONSIBLE_ID": staffID,
                        }
                    },
                    requestId: 'activity-add'
                });
                console.log(JSON.stringify({ 'message': 'Activity add' }));
            } else {
                console.log(JSON.stringify({ 'message': 'Activity not added' }));
            }
        } catch (error) {
            console.error(error);
            console.log(JSON.stringify({ 'message': 'Activity not added: ' + error.message }));
        }
    }

    createCalendarActivity();
    ```

- Python

    ```python
    from datetime import datetime, timedelta

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    contact_id = 1
    result_activity = None

    try:
        contact = client.crm.contact.get(bitrix_id=contact_id).response.result

        if contact.get("ASSIGNED_BY_ID") and contact.get("PHONE"):
            contact_phone = contact["PHONE"][0]
            staff_id = contact["ASSIGNED_BY_ID"]
            now = datetime.now()
            result_activity = client.crm.activity.add(
                fields={
                    "SUBJECT": "calendar title",
                    "DESCRIPTION": "calendar body",
                    "DESCRIPTION_TYPE": 3,
                    "OWNER_ID": contact_id,
                    "OWNER_TYPE_ID": 3,
                    "TYPE_ID": 1,
                    "COMMUNICATIONS": [
                        {
                            "VALUE": contact_phone["VALUE"],
                            "ENTITY_ID": contact_id,
                            "ENTITY_TYPE_ID": 3,
                        }
                    ],
                    "START_TIME": now.isoformat(timespec="seconds"),
                    "END_TIME": (now + timedelta(hours=1)).isoformat(timespec="seconds"),
                    "RESPONSIBLE_ID": staff_id,
                }
            ).response
    except BitrixAPIError as error:
        print({"message": f"Activity not added: {error}"})
    else:
        if result_activity and result_activity.result:
            print({"message": "Activity add"})
        else:
            print({"message": "Activity not added"})
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $contactID = 1;
    try {
        $resultContact = $sb->getCRMScope()->contact()->get($contactID)->contact();
        $resultActivity = null;
        if (!empty($resultContact->ASSIGNED_BY_ID) && !empty($resultContact->PHONE))
        {
            $phones = $resultContact->PHONE;
            $contactPhone = reset($phones);
            $staffID = $resultContact->ASSIGNED_BY_ID;
            $resultActivity = $sb->getCRMScope()->activity()->add(
                [
                    "SUBJECT" => "calendar title",
                    "DESCRIPTION" => "calendar body",
                    "DESCRIPTION_TYPE" => 3,// text type (crm.enum.contenttype): plain, HTML, BB-code
                    "OWNER_ID" => $contactID,
                    "OWNER_TYPE_ID" => 3, // crm.enum.ownertype
                    "TYPE_ID" => 1, // crm.enum.activitytype
                    "COMMUNICATIONS" => [
                        [
                            'VALUE' => $contactPhone->VALUE,
                            'ENTITY_ID' => $contactID,
                            'ENTITY_TYPE_ID' => 3// crm.enum.ownertype
                        ]
                    ],
                    "START_TIME" => date("Y-m-d H:i:s", time()),
                    "END_TIME" => date("Y-m-d H:i:s", time() + 3600),
                    "RESPONSIBLE_ID" => $staffID,
                ]
            )->getId();
        }
        if (!empty($resultActivity))
        {
            echo json_encode(['message' => 'Activity add']);
        }
        else
        {
            echo json_encode(['message' => 'Activity not added']);
        }
    } catch (\Throwable $e) {
        echo json_encode(['message' => 'Activity not added: ' . $e->getMessage()]);
    }
    ```

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    //
    // The example is self-contained: it creates a contact with a phone number, reads its data,
    // creates a calendar event linked to this contact and cleans up after itself.
    // It runs on any portal, nothing needs to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"fmt"
    	"log"
    	"os"
    	"time"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    // entityTypeContact is the ID of the "contact" object type from crm.enum.ownertype.
    const entityTypeContact = 3

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- setup: our own contact with a phone number

    	contactID, err := addContact(ctx, core)
    	if err != nil {
    		return err
    	}
    	defer del(ctx, core, "crm.contact.delete", b24.Params{"id": contactID})

    	// --- step 1: the client data
    	res, err := core.Call(ctx, "crm.contact.get",
    		b24.Params{"id": contactID}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.contact.get: %w", err)
    	}

    	// The phone and the responsible person are needed from the response. PHONE is a multifield: a list of
    	// objects, even when there is a single number, and it arrives only if the contact
    	// has any phone numbers at all.
    	var contact struct {
    		ID           b24.ID `json:"ID"`
    		Name         string `json:"NAME"`
    		LastName     string `json:"LAST_NAME"`
    		AssignedByID b24.ID `json:"ASSIGNED_BY_ID"`
    		Phone        []struct {
    			ID        b24.ID `json:"ID"`
    			Value     string `json:"VALUE"`
    			ValueType string `json:"VALUE_TYPE"`
    		} `json:"PHONE"`
    	}
    	if err := json.Unmarshal(res.Result, &contact); err != nil {
    		return fmt.Errorf("parse contact: %w", err)
    	}
    	if len(contact.Phone) == 0 {
    		return fmt.Errorf("contact %d has no phone number", contactID)
    	}
    	fmt.Printf("contact %d %s %s, phone %s, responsible %d\n",
    		contact.ID, contact.Name, contact.LastName, contact.Phone[0].Value, contact.AssignedByID)

    	// --- step 2: the calendar event
    	// The start and end times are in ISO 8601 format. Here it is a one-hour meeting,
    	// tomorrow at the same time.
    	start := time.Now().Add(24 * time.Hour)

    	res, err = core.Call(ctx, "crm.activity.add", b24.Params{
    		"fields": b24.Params{
    			"SUBJECT":     "calendar title",
    			"DESCRIPTION": "calendar body",
    			// 1 — plain text, 2 — HTML, 3 — BB code.
    			"DESCRIPTION_TYPE": 3,
    			"OWNER_ID":         contact.ID,
    			"OWNER_TYPE_ID":    entityTypeContact,
    			// 1 — a meeting; crm.enum.activitytype returns the full list.
    			"TYPE_ID": 1,
    			// COMMUNICATIONS links the activity to the client's contact details:
    			// the value is taken from the PHONE multifield retrieved in step 1.
    			"COMMUNICATIONS": []b24.Params{{
    				"VALUE":          contact.Phone[0].Value,
    				"ENTITY_ID":      contact.ID,
    				"ENTITY_TYPE_ID": entityTypeContact,
    			}},
    			"START_TIME":     start.Format(time.RFC3339),
    			"END_TIME":       start.Add(time.Hour).Format(time.RFC3339),
    			"RESPONSIBLE_ID": contact.AssignedByID,
    		},
    	})
    	if err != nil {
    		return fmt.Errorf("crm.activity.add: %w", err)
    	}

    	// There is no wrapper: result is the ID of the created activity itself.
    	var activityID b24.ID
    	if err := json.Unmarshal(res.Result, &activityID); err != nil {
    		return fmt.Errorf("parse event ID: %w", err)
    	}
    	defer del(ctx, core, "crm.activity.delete", b24.Params{"id": activityID})

    	fmt.Printf("event %d created for %s\n", activityID, start.Format("02.01.2006 15:04"))
    	return nil
    }

    // --- helpers: data setup and cleanup

    // addContact creates a contact with a phone number: the page takes a ready-made contact with
    // ID 1, but on someone else's portal that is a different person or nobody.
    func addContact(ctx context.Context, core *b24.Core) (b24.ID, error) {
    	res, err := core.Call(ctx, "crm.contact.add", b24.Params{
    		"fields": b24.Params{
    			"NAME":      "Klaus",
    			"LAST_NAME": "Müller",
    			// A multifield: a row without an ID ADDS a value. MultifieldAdd
    			// assembles it for you, so you do not get lost in the keys.
    			"PHONE": []map[string]any{
    				b24.MultifieldAdd("+49 800 100-10-20", "MOBILE"),
    			},
    		},
    	})
    	if err != nil {
    		return 0, fmt.Errorf("crm.contact.add: %w", err)
    	}
    	var id b24.ID
    	return id, json.Unmarshal(res.Result, &id)
    }

    // del removes what was created. A cleanup error is printed but not returned: it must not
    // mask the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, params b24.Params) {
    	if _, err := core.Call(ctx, method, params); err != nil {
    		fmt.Fprintf(os.Stderr, "cleanup, %s: %v
", method, err)
    	}
    }
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-delete.md)
- [{#T}](../../../api-reference/crm/contacts/crm-contact-get.md)
- [{#T}](../../../api-reference/crm/data-types.md)
- [{#T}](how-to-send-email.md)
