# How to Send an E-mail to a Client on Behalf of an Employee

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify a CRM entity

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can automatically send an e-mail to a client through the CRM. The "From" field will display the name and e-mail address of the employee. An event for the outgoing e-mail will be added to the contact card.

To send the e-mail, we will sequentially execute three methods:

1. [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) — retrieve client data

2. [user.get](../../../api-reference/user/user-get.md) — retrieve employee data

3. [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) — create a deal of type "E-mail"

## 1. Retrieve Client Data

We will use the [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) method with the client ID. The ID value can be stored in the variable `contactID`. For example, we will retrieve the contact data with the ID `1`.

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    let contactID = 1;
    BX24.callMethod(
            'crm.contact.get',
            { 'id': contactID },
            function(result) {
                if (result.error()) {
                    reject(result.error());
                } else {
                    resolve(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    $contactID = 1;
    $resultContact = CRest::call(
        'crm.contact.get',
        [
            'id' => $contactID
        ]
    );
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            auth_token="your-webhook-token",
        )
    )

    contact_id = 1
    result_contact = client.crm.contact.get(
        bitrix_id=contact_id,
    ).response.result
    ```

{% endlist %}

As a result, we will obtain client data, including the e-mail address `EMAIL` and the ID of the responsible employee `ASSIGNED_BY_ID`.

```json
{
    "result": {
        "ID": "1",
        "NAME": "Alex",
        "SECOND_NAME": "Kirillovich",
        "LAST_NAME": "Vronsky",
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

To get the data of the responsible employee, we will use the [user.get](../../../api-reference/user/user-get.md) method with a filter by employee ID. The ID should take the value from the `ASSIGNED_BY_ID` field of the `resultContact` object.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'user.get',
        {
            'filter': {
                'ID': resultContact.ASSIGNED_BY_ID
            }
        },
        function(result) {
            if (result.error()) {
                reject(result.error());
            } else {
                 resolve(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    {
        $resultUser = CRest::call(
            'user.get',
            [
                'filter' => [
                    'ID' => $resultContact['result']['ASSIGNED_BY_ID']
                ]
            ]
       );
    }
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

We will obtain employee data, including the e-mail address `EMAIL`.

```json
{
    "result": [
        {
        "ID": "61",
        "ACTIVE": true,
        "NAME": "Ivan",
        "LAST_NAME": "Petrov",
        "EMAIL": "ivanpetrov@example.com"
        }
    ]
}
```

## 3. Create a Deal of Type "E-mail"

We will prepare the variables:

- `contactEmail` — the first element from the contact `resultContact`,

- `staff` — the first element from the `resultUser` object.

{% list tabs %}

- JS

    ```js
        let contactEmail = resultContact.EMAIL[0];
        let staff = resultUser[0];
    ```

- PHP

    ```php
        $contactEmail = reset($resultContact['result']['EMAIL']);
        $staff = reset($resultUser['result']);
    ```

- Python

    ```python
    contact_email = result_contact["EMAIL"][0]
    staff = result_user[0]
    ```

{% endlist %}

To add the event and send the e-mail, we will use the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method. We need to pass the client data, employee data, and deal parameters.

- `SUBJECT` — the subject of the e-mail. We will specify `subject email now`.

- `DESCRIPTION` — the body of the e-mail. For example, `body email now`.

- `DESCRIPTION_TYPE` — the text type. Possible values: `1`— plain text, `2`— HTML markup, `3`— BB-code. We will set the value to `3`.

- `COMPLETED` — a flag indicating whether the event is completed. We will specify `Y`.

- `DIRECTION` — the direction of the activity. We will pass `2` — outgoing e-mail. A complete list of activity directions can be obtained using the [crm.enum.activitydirection](../../../api-reference/crm/auxiliary/enum/outdated/crm-enum-activity-direction.md) method.

- `OWNER_ID` — the contact ID. We will pass the variable `contactID`.

- `OWNER_TYPE_ID` — [the identifier of the CRM object type](../../../api-reference/crm/data-types.md#object_type). We will pass `3`— contact. A complete list of object types can be obtained using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method.

- `TYPE_ID` — the activity type. We will specify `4` — e-mail. A list of activity types can be obtained using the [crm.enum.activitytype](../../../api-reference/crm/auxiliary/enum/outdated/crm-enum-activity-type.md) method.

- `COMMUNICATIONS` — the client's contact details:

    - `VALUE` — the e-mail address, we will take the `VALUE` from the `contactEmail` array,

    - `ENTITY_ID` — the client ID, we will pass `contactID`,

    - `ENTITY_TYPE_ID` — [the identifier of the object type](../../../api-reference/crm/data-types.md#object_type), we will pass `3` — contact.

- `START_TIME` and `END_TIME` — the start and end date and time of the activity. We will specify a duration of 1 hour.

- `RESPONSIBLE_ID` — the ID of the responsible person, we will pass `staff.ID`.

- `SETTINGS` — additional settings:

    - `MESSAGE_FROM` — the sender of the e-mail, we will pass the name `staff.NAME`, last name `staff.LAST_NAME`, and e-mail address `staff.EMAIL` of the employee.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'crm.activity.add',
        {
            'fields': {
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
        }
    ); 
    ```

- PHP

    ```php
    $resultActivity = CRest::call(
        'crm.activity.add',
        [
            'fields' => [
                "SUBJECT" => "subject email now",
                "DESCRIPTION" => "body email now",
                "DESCRIPTION_TYPE" => 3,//text,html,bbCode type id in: CRest::call('crm.enum.contenttype');
                "COMPLETED" => "Y",//send now
                "DIRECTION" => 2,// CRest::call('crm.enum.activitydirection');
                "OWNER_ID" => $contactID,
                "OWNER_TYPE_ID" => 3, // CRest::call('crm.enum.ownertype');
                "TYPE_ID" => 4, // CRest::call('crm.enum.activitytype');
                "COMMUNICATIONS" => [
                    [
                        'VALUE' => $contactEmail['VALUE'],
                        'ENTITY_ID' => $contactID,
                        'ENTITY_TYPE_ID' => 3// CRest::call('crm.enum.ownertype');
                    ]
                ],
                "START_TIME" => date("Y-m-d H:i:s", time()),
                "END_TIME" => date("Y-m-d H:i:s", time() + 3600),
                "RESPONSIBLE_ID" => $staff['ID'],
                'SETTINGS' => [
                    'MESSAGE_FROM' => implode(
                        ' ',
                        [$staff['NAME'], $staff['LAST_NAME'], '<' . $staff['EMAIL'] . '>']
                    ),
                ],
            ]
        ]
    );
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

If the event is created successfully, the method will return its ID. If you receive an `error`, refer to the documentation for the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method to understand possible errors.

```json
{
    "result": 3165,
}
```

## Complete Code Example

The code in this example combines all steps: it retrieves client and employee data, adds the "E-mail" deal, and sends the e-mail to the client.

{% list tabs %}

- JS

    ```js
    document.addEventListener('DOMContentLoaded', function() {
        async function createEmailActivityForContact() {
            try {
                let contactID = 1;

                let resultContact = await new Promise((resolve, reject) => {
                    BX24.callMethod(
                        'crm.contact.get',
                        { 'id': contactID },
                        function(result) {
                            if (result.error()) {
                                reject(result.error());
                            } else {
                                resolve(result.data());
                            }
                        }
                    );
                });

                if (resultContact && resultContact.ASSIGNED_BY_ID && resultContact.EMAIL) {
                    let resultUser = await new Promise((resolve, reject) => {
                        BX24.callMethod(
                            'user.get',
                            {
                                'filter': {
                                    'ID': resultContact.ASSIGNED_BY_ID
                                }
                            },
                            function(result) {
                                if (result.error()) {
                                    reject(result.error());
                                } else {
                                    resolve(result.data());
                                }
                            }
                        );
                    });

                    if (resultUser.length > 0) {
                        let contactEmail = resultContact.EMAIL[0];
                        let staff = resultUser[0];

                        if (contactEmail.VALUE && staff.EMAIL) {
                            let resultActivity = await new Promise((resolve, reject) => {
                                BX24.callMethod(
                                    'crm.activity.add',
                                    {
                                        'fields': {
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
                                    function(result) {
                                        if (result.error()) {
                                            reject(result.error());
                                        } else {
                                            resolve(result.data());
                                        }
                                    }
                                );
                            });

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
    });
    ```

- PHP

    ```php
    <?php
    $contactID = 1;
    $resultContact = CRest::call(
        'crm.contact.get',
        [
            'id' => $contactID
        ]
    );
    $resultActivity = [];
    if (!empty($resultContact['result']['ASSIGNED_BY_ID']) && !empty($resultContact['result']['EMAIL']))
    {
        $resultUser = CRest::call(
            'user.get',
            [
                'filter' => [
                    'ID' => $resultContact['result']['ASSIGNED_BY_ID']
                ]
            ]
        );
        if ($resultUser['result'])
        {
            $contactEmail = reset($resultContact['result']['EMAIL']);
            $staff = reset($resultUser['result']);
            if (!empty($contactEmail['VALUE']) && !empty($staff['EMAIL']))
            {
                $resultActivity = CRest::call(
                    'crm.activity.add',
                    [
                        'fields' => [
                            "SUBJECT" => "subject email now",
                            "DESCRIPTION" => "body email now",
                            "DESCRIPTION_TYPE" => 3,//text,html,bbCode type id in: CRest::call('crm.enum.contenttype');
                            "COMPLETED" => "Y",//send now
                            "DIRECTION" => 2,// CRest::call('crm.enum.activitydirection');
                            "OWNER_ID" => $contactID,
                            "OWNER_TYPE_ID" => 3, // CRest::call('crm.enum.ownertype');
                            "TYPE_ID" => 4, // CRest::call('crm.enum.activitytype');
                            "COMMUNICATIONS" => [
                                [
                                    'VALUE' => $contactEmail['VALUE'],
                                    'ENTITY_ID' => $contactID,
                                    'ENTITY_TYPE_ID' => 3// CRest::call('crm.enum.ownertype');
                                ]
                            ],
                            "START_TIME" => date("Y-m-d H:i:s", time()),
                            "END_TIME" => date("Y-m-d H:i:s", time() + 3600),
                            "RESPONSIBLE_ID" => $staff['ID'],
                            'SETTINGS' => [
                                'MESSAGE_FROM' => implode(
                                    ' ',
                                    [$staff['NAME'], $staff['LAST_NAME'], '<' . $staff['EMAIL'] . '>']
                                ),
                            ],
                        ]
                    ]
                );
            }
        }
    }
    if (!empty($resultActivity['result']))
    {
        echo json_encode(['message' => 'Activity added']);
    }
    elseif (!empty($resultActivity['error_description']))
    {
        echo json_encode(['message' => 'Activity not added: ' . $resultActivity['error_description']]);
    }
    else
    {
        echo json_encode(['message' => 'Activity not added']);
    }
    ?>
    ```

- Python

    ```python
    from datetime import datetime, timedelta

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            auth_token="your-webhook-token",
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
            print({"message": "Activity added"})
        else:
            print({"message": "Activity not added"})
    except BitrixAPIError as error:
        print({"message": f"Activity not added: {error}"})
    ```

{% endlist %}