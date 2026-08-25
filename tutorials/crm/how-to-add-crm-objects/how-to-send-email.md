# How to Send an E-mail to a Client on Behalf of an Employee

> Scope: [`crm`, `user_basic`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — permission to modify the contact
>
> - [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) — a user with permission to read contacts
> - [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) — a user with permission to modify the CRM object the activity is added to
> - [user.get](../../../api-reference/user/user-get.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can automatically send an e-mail to a client through the CRM. The "From" field will display the name and e-mail address of the employee. An "E-mail" activity will be added to the contact card.

An e-mail can also be sent through REST with the [mail.message.send](../../../api-reference/mail/message/mail-message-send.md) method, but it works with a mailbox rather than with the CRM: the e-mail is sent, yet nothing about it remains in the client card. Here the task is the opposite — the e-mail has to land in the contact timeline, and only [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) creates such an entry.

Sending happens as a side effect of creating the activity. Bitrix24 sends the e-mail only if three conditions come together in a single request — `TYPE_ID` with the value `4`, `DIRECTION` with the value `2`, and `COMPLETED` with the value `Y`. If at least one of them is missing, the activity appears in the card, but the e-mail is not sent.

The method that creates the activity does not retrieve the participants' data on its own: the client address for `COMMUNICATIONS` and the sender signature for `SETTINGS` have to be retrieved from the contact card and the employee profile first. That is why the scenario consists of three steps.

1. Retrieve the client address and the person responsible for the client using the [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) method

2. Retrieve the name and the address of the responsible person using the [user.get](../../../api-reference/user/user-get.md) method

3. Create the activity using the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method, building `COMMUNICATIONS` and `SETTINGS` from the retrieved values

As a result, the method returns the activity identifier, the activity appears in the contact timeline, and the e-mail is sent to the client address.

## Before You Start

- The contact is already created in Bitrix24, and you know its identifier. The identifier is returned by the [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) and [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) methods

- The contact has an e-mail address filled in. Without an address in `COMMUNICATIONS` the e-mail has nowhere to go, and the method returns the `Email send error. "To" is not found` error

- The contact has the "Responsible person" field filled in, and the employee has an e-mail address. The "From" field is built from them

- The webhook or the application has access to the `user_basic` or `user` scope. The `user_brief` scope will not do: it returns user data without contact details, so `EMAIL` does not arrive in the [user.get](../../../api-reference/user/user-get.md) response

- The webhook is created on behalf of a user who can modify this contact. The method checks permissions not on the activity, but on the CRM object the activity is attached to

## 1. Retrieve Client Data

Use the [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) method with the customer identifier. We store the identifier in the `contactID` variable — it is needed once again in step 3. Replace `1` with the identifier of your own contact.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    let contactID = 1;
    const response = await $b24.actions.v2.call.make({
        method: 'crm.contact.get',
        params: { id: contactID },
        requestId: 'contact-get'
    })
    let resultContact = response.getData().result
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
{% endlist %}

As a result, we will retrieve the customer data. Save two values for the next steps:

- `EMAIL[0].VALUE` — the e-mail address. This is a multifield: the method returns a list of objects even when there is a single address, and it does not return the `EMAIL` key at all if the contact has no addresses

- `ASSIGNED_BY_ID` — the identifier of the responsible employee, step 2 runs on it

The response is shortened to the fields the scenario needs.

```json
{
    "result": {
        "ID": "1",
        "NAME": "Klaus",
        "SECOND_NAME": "Werner",
        "LAST_NAME": "Müller",
        "ASSIGNED_BY_ID": "61",
        "EMAIL": [
            {
                "ID": "1328",
                "VALUE_TYPE": "WORK",
                "VALUE": "mueller@example.com",
                "TYPE_ID": "EMAIL"
            }
        ]
    }
}
```

## 2. Retrieve Employee Data

To retrieve the data of the responsible employee, use the [user.get](../../../api-reference/user/user-get.md) method with a filter by identifier. We pass the `ASSIGNED_BY_ID` value from the response of step 1 into the `ID` filter.

{% list tabs %}

- JS

    ```js
    const responseUser = await $b24.actions.v2.call.make({
        method: 'user.get',
        params: {
            filter: {
                ID: resultContact.ASSIGNED_BY_ID
            }
        },
        requestId: 'user-get'
    })
    let resultUser = responseUser.getData().result
    ```

- Python

    ```python
    result_user = client.user.get(
        filter={
            "ID": result_contact["ASSIGNED_BY_ID"],
        }
    ).response.result
    ```


- PHP

    ```php
    $resultUser = $sb->getUserScope()->user()->get(
        [],
        ['ID' => $resultContact->ASSIGNED_BY_ID]
    )->getUsers();
    ```
{% endlist %}

The method returns a list even when the filter selects a single user. Three fields from the first item are needed — `NAME`, `LAST_NAME`, and `EMAIL`: the "From" field is built from them. The `EMAIL` field of a user is a plain string, not a multifield as it is for a contact.

```json
{
    "result": [
        {
            "ID": "61",
            "ACTIVE": true,
            "NAME": "Hans",
            "LAST_NAME": "Weber",
            "EMAIL": "weber@example.com"
        }
    ]
}
```

## 3. Create an E-mail Activity

Prepare the variables:

- `contactEmail` — the first item of the `EMAIL` multifield from the response of step 1

- `staff` — the first item of the list from the response of step 2

{% list tabs %}

- JS

    ```js
    let contactEmail = resultContact.EMAIL[0];
    let staff = resultUser[0];
    ```

- Python

    ```python
    contact_email = result_contact["EMAIL"][0]
    staff = result_user[0]
    ```


- PHP

    ```php
    $emails = $resultContact->EMAIL;
    $contactEmail = reset($emails);
    $staff = reset($resultUser);
    ```
{% endlist %}

To add the activity and send the e-mail, use the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method with the following parameters:

- `SUBJECT` — the subject of the e-mail. We will specify `subject email now`. The method does not accept an empty string

- `DESCRIPTION` — the e-mail body. For example, `body email now`

- `DESCRIPTION_TYPE` — the text format: `1` — plain text, `2` — HTML markup, `3` — BB-code. Set the value `3`

- `COMPLETED` — the marker of a completed activity. We will specify `Y`. This is one of the three conditions for sending: with `N` the activity remains a scheduled e-mail, and Bitrix24 does not send it

- `DIRECTION` — the activity direction: `1` — inbound, `2` — outbound. We pass `2` — the second condition for sending

- `OWNER_ID` — the identifier of the CRM object in whose card the activity appears. We pass the variable `contactID`

- `OWNER_TYPE_ID` — the [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). We pass `3` — contact. A full list of object types is returned by the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method

- `TYPE_ID` — the activity type. We will specify `4` — e-mail, this is the third condition for sending. The method accepts only `1` — meeting, `2` — call, `4` — e-mail, and `6` — an activity of an external provider

- `COMMUNICATIONS` — client contact details, the recipient address is taken from here:

    - `VALUE` — the e-mail address, we take `contactEmail.VALUE`

    - `ENTITY_ID` — the client identifier, we pass `contactID`

    - `ENTITY_TYPE_ID` — the [object type identifier](../../../api-reference/crm/data-types.md#object_type), we pass `3` — contact

- `START_TIME` and `END_TIME` — the activity start and end date and time. We will specify a duration of one hour

- `RESPONSIBLE_ID` — the responsible person identifier, we pass `staff.ID`

- `SETTINGS` — additional settings:

    - `MESSAGE_FROM` — the "From" field. We build the string from the name `staff.NAME`, the surname `staff.LAST_NAME`, and the address `staff.EMAIL` in the `Name Surname <address>` format. The address is required: Bitrix24 takes it from the angle brackets and validates it as an e-mail address

We do not pass the `TYPE` key in `COMMUNICATIONS`: for an e-mail Bitrix24 substitutes `EMAIL` on its own. If you pass it manually with a different value, the communication is discarded and the e-mail has nowhere to go.

{% note warning "" %}

The next request sends an e-mail to the real address from `COMMUNICATIONS`. Sending cannot be cancelled: deleting the activity does not recall the e-mail. Debug the scenario on a test contact with your own address.

{% endnote %}

{% list tabs %}

- JS

    ```js
    const responseActivity = await $b24.actions.v2.call.make({
        method: 'crm.activity.add',
        params: {
            fields: {
                "SUBJECT": "subject email now",
                "DESCRIPTION": "body email now",
                "DESCRIPTION_TYPE": 3,
                "COMPLETED": "Y",
                "DIRECTION": 2,
                "OWNER_ID": contactID,
                "OWNER_TYPE_ID": 3,
                "TYPE_ID": 4,
                "COMMUNICATIONS": [
                    {
                        'VALUE': contactEmail.VALUE,
                        'ENTITY_ID': contactID,
                        'ENTITY_TYPE_ID': 3
                    }
                ],
                "START_TIME": new Date().toISOString(),
                "END_TIME": new Date(Date.now() + 3600 * 1000).toISOString(),
                "RESPONSIBLE_ID": staff.ID,
                'SETTINGS': {
                    'MESSAGE_FROM': `${staff.NAME} ${staff.LAST_NAME} <${staff.EMAIL}>`
                }
            }
        },
        requestId: 'activity-add'
    });
    ```

- Python

    ```python
    from datetime import datetime, timedelta

    now = datetime.now()

    result_activity = client.crm.activity.add(
        fields={
            "SUBJECT": "subject email now",
            "DESCRIPTION": "body email now",
            "DESCRIPTION_TYPE": 3,
            "COMPLETED": "Y",
            "DIRECTION": 2,
            "OWNER_ID": contact_id,
            "OWNER_TYPE_ID": 3,
            "TYPE_ID": 4,
            "COMMUNICATIONS": [
                {
                    "VALUE": contact_email["VALUE"],
                    "ENTITY_ID": contact_id,
                    "ENTITY_TYPE_ID": 3,
                }
            ],
            "START_TIME": now.isoformat(timespec="seconds"),
            "END_TIME": (now + timedelta(hours=1)).isoformat(timespec="seconds"),
            "RESPONSIBLE_ID": staff["ID"],
            "SETTINGS": {
                "MESSAGE_FROM": f"{staff['NAME']} {staff['LAST_NAME']} <{staff['EMAIL']}>"
            },
        }
    ).response.result
    ```


- PHP

    ```php
    $resultActivity = $sb->getCRMScope()->activity()->add(
        [
            "SUBJECT" => "subject email now",
            "DESCRIPTION" => "body email now",
            "DESCRIPTION_TYPE" => 3,// text type (crm.enum.contenttype): plain, HTML, BB-code
            "COMPLETED" => "Y",// send now
            "DIRECTION" => 2,// crm.enum.activitydirection
            "OWNER_ID" => $contactID,
            "OWNER_TYPE_ID" => 3, // crm.enum.ownertype
            "TYPE_ID" => 4, // crm.enum.activitytype
            "COMMUNICATIONS" => [
                [
                    'VALUE' => $contactEmail->VALUE,
                    'ENTITY_ID' => $contactID,
                    'ENTITY_TYPE_ID' => 3// crm.enum.ownertype
                ]
            ],
            "START_TIME" => date("Y-m-d H:i:s", time()),
            "END_TIME" => date("Y-m-d H:i:s", time() + 3600),
            "RESPONSIBLE_ID" => $staff->ID,
            'SETTINGS' => [
                'MESSAGE_FROM' => implode(
                    ' ',
                    [$staff->NAME, $staff->LAST_NAME, '<' . $staff->EMAIL . '>']
                ),
            ],
        ]
    )->getId();
    ```
{% endlist %}

We have created the activity and received its identifier `3165` in response. There is no wrapper in the response: `result` is the number itself. The identifier can be used in the methods for [updating](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md) and [deleting](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-delete.md) an activity.

```json
{
    "result": 3165
}
```

A successful response confirms that the activity was created, but not that the e-mail was delivered. The method returns sending errors as separate codes — they are collected in the "Errors and Diagnostics" section.

## Verify the Result

Open the contact card in Bitrix24. The e-mail is displayed in the card timeline, and the "From" field holds the name and the address of the employee from `MESSAGE_FROM`. Check the client mailbox: the e-mail arrives with the subject from `SUBJECT` and the body from `DESCRIPTION`.

Through REST, the contact activities are returned by the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method with the same `OWNER_TYPE_ID` and `OWNER_ID` values as in step 3. The `COMMUNICATIONS` field is returned only when it is specified in `select`.

{% list tabs %}

- JS

    ```js
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

The scenario is complete if the response contains an object with the `ID` from step 3, its `TYPE_ID` equals `4`, `DIRECTION` equals `2`, `COMPLETED` equals `Y`, and `COMMUNICATIONS` holds the client address with the `EMAIL` type.

```json
{
    "result": [
        {
            "ID": "3165",
            "OWNER_ID": "1",
            "OWNER_TYPE_ID": "3",
            "TYPE_ID": "4",
            "SUBJECT": "subject email now",
            "COMPLETED": "Y",
            "DIRECTION": "2",
            "RESPONSIBLE_ID": "61",
            "DESCRIPTION": "body email now",
            "DESCRIPTION_TYPE": "3",
            "COMMUNICATIONS": [
                {
                    "ID": "4488",
                    "TYPE": "EMAIL",
                    "VALUE": "mueller@example.com",
                    "ENTITY_ID": "1",
                    "ENTITY_TYPE_ID": "3"
                }
            ]
        }
    ],
    "total": 1
}
```

In the response, numeric fields arrive as strings — `"TYPE_ID": "4"`, even though a number was passed in the request. This is not a sign of an error.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `Access denied.` | The user does not have permission to modify the contact from `OWNER_ID`. Check which user the webhook was created on behalf of ||
|| `Could not find 'CONTACT' with ID: 1` | There is no contact with such `OWNER_ID` in Bitrix24. Take an existing identifier using the [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) method ||
|| `The field SUBJECT is not defined or empty` | An empty string was passed in `SUBJECT`, or the field was omitted ||
|| `The field COMMUNICATIONS is not defined or invalid` | The communication was not passed or was discarded. This happens when the contact has no address and an empty value ends up in `VALUE`, or when a value other than `EMAIL` was passed in `TYPE`. Check `EMAIL` in the response of step 1 ||
|| `Email send error. "To" is not found` | The activity was created, the e-mail was not sent: there is no valid recipient address among the communications. Check that `VALUE` holds an address and not an empty string ||
|| `Email send error. "From" is not found` | The activity was created, the e-mail was not sent: the sender could not be determined. `SETTINGS.MESSAGE_FROM` is empty and the employee has no mailbox connected to the CRM. Check `EMAIL` in the response of step 2 ||
|| `Email send error. Invalid email is specified` | The sender or the recipient address failed the format check. Build `MESSAGE_FROM` as `Name Surname <address>` and check the client address ||
|| `Email send error. Failed to load module "subscribe"` | The mailing module the e-mail is sent through is not installed in Bitrix24. The activity was created, the e-mail was not sent ||
|#

Repeat the scenario from the step that returned the error. Steps 1 and 2 do not create anything, so they can be executed any number of times. Errors with the `Email send error` prefix mean that the activity has already been created while the e-mail did not go out: before repeating, delete the created activity using the [crm.activity.delete](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-delete.md) method, otherwise the contact card keeps an e-mail the client never received.

A separate case is when the method returns an identifier without errors, the activity is present in the card, but the e-mail did not arrive. Check that the request contained `TYPE_ID` with the value `4`, `DIRECTION` with the value `2`, and `COMPLETED` with the value `Y` at the same time. Without any of the three, Bitrix24 creates the activity silently and does not start the sending.

## Key Considerations

- A copy of the e-mail is sent to the address from `MESSAGE_FROM`. To turn it off, pass the `DISABLE_SENDING_MESSAGE_COPY` key with the value `Y` in `SETTINGS`

- If `MESSAGE_FROM` is not passed, Bitrix24 substitutes the sender on its own: first the employee mailbox connected to the CRM, then the shared CRM mailbox. When there is neither, the method returns `Email send error. "From" is not found`

- Running the example again creates one more activity and sends the client one more e-mail, duplicates are not filtered out

- The [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method is no longer being developed, but there is no replacement for this scenario: it is the only method that both sends the e-mail and records it in the client card with a single call. The [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) method creates a scheduled activity and does not send e-mails

- If the CRM entry is not needed, take [mail.message.send](../../../api-reference/mail/message/mail-message-send.md) from the mail section: it sends an e-mail from a connected mailbox, supports carbon copy and blind carbon copy, and respects the sending limits. It cannot be chained into "send and attach to the contact" — the method does not return the message identifier, and [mail.message.createCrmActivity](../../../api-reference/mail/message/mail-message-createcrmactivity.md) requires it as input

## Code Example

The example combines all three steps: it retrieves the customer and employee data, adds an "E-mail" activity, and sends an e-mail to the customer. Replace `contactID` with the identifier of your own contact, and `SUBJECT` and `DESCRIPTION` — with your own text.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    async function createEmailActivityForContact() {
        try {
            let contactID = 1;

            const responseContact = await $b24.actions.v2.call.make({
                method: 'crm.contact.get',
                params: { id: contactID },
                requestId: 'contact-get'
            });
            let resultContact = responseContact.getData().result;

            if (resultContact && resultContact.ASSIGNED_BY_ID && resultContact.EMAIL) {
                const responseUser = await $b24.actions.v2.call.make({
                    method: 'user.get',
                    params: { filter: { ID: resultContact.ASSIGNED_BY_ID } },
                    requestId: 'user-get'
                });
                let resultUser = responseUser.getData().result;

                if (resultUser.length > 0) {
                    let contactEmail = resultContact.EMAIL[0];
                    let staff = resultUser[0];

                    if (contactEmail.VALUE && staff.EMAIL) {
                        const responseActivity = await $b24.actions.v2.call.make({
                            method: 'crm.activity.add',
                            params: {
                                fields: {
                                    "SUBJECT": "subject email now",
                                    "DESCRIPTION": "body email now",
                                    "DESCRIPTION_TYPE": 3,
                                    "COMPLETED": "Y",
                                    "DIRECTION": 2,
                                    "OWNER_ID": contactID,
                                    "OWNER_TYPE_ID": 3,
                                    "TYPE_ID": 4,
                                    "COMMUNICATIONS": [
                                        {
                                            'VALUE': contactEmail.VALUE,
                                            'ENTITY_ID': contactID,
                                            'ENTITY_TYPE_ID': 3
                                        }
                                    ],
                                    "START_TIME": new Date().toISOString(),
                                    "END_TIME": new Date(Date.now() + 3600 * 1000).toISOString(),
                                    "RESPONSIBLE_ID": staff.ID,
                                    'SETTINGS': {
                                        'MESSAGE_FROM': `${staff.NAME} ${staff.LAST_NAME} <${staff.EMAIL}>`
                                    }
                                }
                            },
                            requestId: 'activity-add'
                        });
                        let resultActivity = responseActivity.getData().result;

                        if (resultActivity) {
                            console.log(JSON.stringify({ 'message': 'Activity added' }));
                        } else {
                            console.log(JSON.stringify({ 'message': 'Activity not added' }));
                        }
                    }
                }
            }
        } catch (error) {
            console.error(error);
            console.log(JSON.stringify({ 'message': 'Activity not added: ' + error.message }));
        }
    }

    createEmailActivityForContact();
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

    try:
        contact = client.crm.contact.get(bitrix_id=contact_id).response.result
        result_activity = None

        if contact.get("ASSIGNED_BY_ID") and contact.get("EMAIL"):
            result_user = client.user.get(
                filter={"ID": contact["ASSIGNED_BY_ID"]},
            ).response.result

            if result_user:
                contact_email = contact["EMAIL"][0]
                staff = result_user[0]

                if contact_email.get("VALUE") and staff.get("EMAIL"):
                    now = datetime.now()
                    result_activity = client.crm.activity.add(
                        fields={
                            "SUBJECT": "subject email now",
                            "DESCRIPTION": "body email now",
                            "DESCRIPTION_TYPE": 3,
                            "COMPLETED": "Y",
                            "DIRECTION": 2,
                            "OWNER_ID": contact_id,
                            "OWNER_TYPE_ID": 3,
                            "TYPE_ID": 4,
                            "COMMUNICATIONS": [
                                {
                                    "VALUE": contact_email["VALUE"],
                                    "ENTITY_ID": contact_id,
                                    "ENTITY_TYPE_ID": 3,
                                }
                            ],
                            "START_TIME": now.isoformat(timespec="seconds"),
                            "END_TIME": (now + timedelta(hours=1)).isoformat(timespec="seconds"),
                            "RESPONSIBLE_ID": staff["ID"],
                            "SETTINGS": {
                                "MESSAGE_FROM": f"{staff['NAME']} {staff['LAST_NAME']} <{staff['EMAIL']}>"
                            },
                        }
                    ).response.result

        if result_activity:
            print({"message": "Activity add"})
        else:
            print({"message": "Activity not added"})
    except BitrixAPIError as error:
        print({"message": f"Activity not added: {error}"})
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
        if (!empty($resultContact->ASSIGNED_BY_ID) && !empty($resultContact->EMAIL))
        {
            $resultUser = $sb->getUserScope()->user()->get(
                [],
                ['ID' => $resultContact->ASSIGNED_BY_ID]
            )->getUsers();
            if ($resultUser)
            {
                $emails = $resultContact->EMAIL;
                $contactEmail = reset($emails);
                $staff = reset($resultUser);
                if (!empty($contactEmail->VALUE) && !empty($staff->EMAIL))
                {
                    $resultActivity = $sb->getCRMScope()->activity()->add(
                        [
                            "SUBJECT" => "subject email now",
                            "DESCRIPTION" => "body email now",
                            "DESCRIPTION_TYPE" => 3,// text type (crm.enum.contenttype): plain, HTML, BB-code
                            "COMPLETED" => "Y",// send now
                            "DIRECTION" => 2,// crm.enum.activitydirection
                            "OWNER_ID" => $contactID,
                            "OWNER_TYPE_ID" => 3, // crm.enum.ownertype
                            "TYPE_ID" => 4, // crm.enum.activitytype
                            "COMMUNICATIONS" => [
                                [
                                    'VALUE' => $contactEmail->VALUE,
                                    'ENTITY_ID' => $contactID,
                                    'ENTITY_TYPE_ID' => 3// crm.enum.ownertype
                                ]
                            ],
                            "START_TIME" => date("Y-m-d H:i:s", time()),
                            "END_TIME" => date("Y-m-d H:i:s", time() + 3600),
                            "RESPONSIBLE_ID" => $staff->ID,
                            'SETTINGS' => [
                                'MESSAGE_FROM' => implode(
                                    ' ',
                                    [$staff->NAME, $staff->LAST_NAME, '<' . $staff->EMAIL . '>']
                                ),
                            ],
                        ]
                    )->getId();
                }
            }
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
{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-delete.md)
- [{#T}](../../../api-reference/crm/contacts/crm-contact-get.md)
- [{#T}](../../../api-reference/user/user-get.md)
- [{#T}](../../../api-reference/crm/data-types.md)
- [{#T}](how-to-add-activity-to-contact.md)
