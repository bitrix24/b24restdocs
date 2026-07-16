# How to Send an E-mail to a Client on Behalf of an Employee

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify a CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can automatically send an e-mail to a client through the CRM. The "From" field will display the name and e-mail address of the employee. An event for the outgoing e-mail will be added to the contact card.

To send an e-mail, we will sequentially execute three methods:

1. [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) — retrieve customer data

2. [user.get](../../../api-reference/user/user-get.md) — retrieve employee data

3. [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) — create an activity of type "E-mail"

## 1. Retrieve Client Data

Use the [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) method with the customer identifier. The identifier value can be previously stored in the `contactID` variable. For example, we get contact data with identifier `1`.

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

- PHP

    ```php
    $contactID = 1;
    $resultContact = $sb->getCRMScope()->contact()->get($contactID)->contact();
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

{% endlist %}

As a result, we will retrieve the customer data, including the e-mail address `EMAIL` and the responsible employee identifier `ASSIGNED_BY_ID`.

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
                "VALUE": "vronsky@example.com",
                "TYPE_ID": "EMAIL"
            }
        ]
    } 
}
```

## 2. Retrieve Employee Data

To retrieve the data of the responsible employee, use the [user.get](../../../api-reference/user/user-get.md) method with a filter by the employee identifier. The identifier must take the value from the `ASSIGNED_BY_ID` field of the `resultContact` object.

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

- PHP

    ```php
    $resultUser = $sb->getUserScope()->user()->get(
        [],
        ['ID' => $resultContact->ASSIGNED_BY_ID]
    )->getUsers();
    ```

- Python

    ```python
    result_user = client.user.get(
        filter={
            "ID": result_contact["ASSIGNED_BY_ID"],
        }
    ).response.result
    ```

{% endlist %}

We will retrieve the employee data, including the e-mail address `EMAIL`.

```json
{
    "result": [
        {
        "ID": "61",
        "ACTIVE": true,
        "NAME": "Hans",
        "LAST_NAME": "Weber",
        "EMAIL": "ivanpetrov@example.com"
        }
    ]
}
```

## 3. Create an E-mail Activity

Prepare the variables:

- `contactEmail` — the first item from contact `resultContact`,

- `staff` — the first item from object `resultUser`.

{% list tabs %}

- JS

    ```js
        let contactEmail = resultContact.EMAIL[0];
        let staff = resultUser[0];
    ```

- PHP

    ```php
        $emails = $resultContact->EMAIL;
        $contactEmail = reset($emails);
        $staff = reset($resultUser);
    ```

- Python

    ```python
    contact_email = result_contact["EMAIL"][0]
    staff = result_user[0]
    ```

{% endlist %}

To add an event and send an e-mail, use the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method. You need to pass the customer data, employee data, and activity parameters to it.

- `SUBJECT` — the subject of the e-mail. We will specify `subject email now`.

- `DESCRIPTION` — the e-mail body. For example, `body email now`.

- `DESCRIPTION_TYPE` — the text type. Possible values: `1`— plain text, `2`— HTML markup, `3`— BB-code. Set the value `3`.

- `COMPLETED` — a flag indicating whether the event is completed. We will specify `Y`.

- `DIRECTION` — the activity direction. We pass `2` — an outbound e-mail. A full list of activity directions can be retrieved using the [crm.enum.activitydirection](../../../api-reference/crm/auxiliary/enum/outdated/crm-enum-activity-direction.md) method.

- `OWNER_ID` — the contact identifier. We pass the variable `contactID`.

- `OWNER_TYPE_ID` — the [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). We pass `3`— contact. A full list of object types can be retrieved using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method.

- `TYPE_ID` — the activity type. We will specify `4` — e-mail. A list of activity types can be retrieved using the [crm.enum.activitytype](../../../api-reference/crm/auxiliary/enum/outdated/crm-enum-activity-type.md) method.

- `COMMUNICATIONS` — client contact details:

    - `VALUE` — the e-mail address; we take the value `VALUE` from the `contactEmail` array,

    - `ENTITY_ID` — the customer identifier; we pass `contactID`,

    - `ENTITY_TYPE_ID` — the [object type identifier](../../../api-reference/crm/data-types.md#object_type); we pass `3` — contact.

- `START_TIME` and `END_TIME` — the activity start and end date and time. We will specify a duration of 1 hour.

- `RESPONSIBLE_ID` — the responsible person identifier; we pass `staff.ID`.

- `SETTINGS` — additional settings:

    - `MESSAGE_FROM` — the e-mail sender; we pass the name `staff.NAME`, surname `staff.LAST_NAME`, and e-mail address `staff.EMAIL` of the employee.

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

-  PHP

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

- Python

    ```python
    from datetime import datetime, timedelta

    contact_email = result_contact["EMAIL"][0]
    staff = result_user[0]
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

{% endlist %}

If the event is created successfully, the method will return its identifier. If you receive error `error`, review the possible error descriptions in the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method documentation.

```json
{
    "result": 3165,
}
```

## Full Code Example

The code in this example combines all steps: it retrieves customer and employee data, adds an "E-mail" activity, and sends an e-mail to the customer.

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

{% endlist %}
