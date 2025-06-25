# How to Send an Email to a Client on Behalf of an Employee

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can perform the method: users with permission to modify a CRM entity

An email can be sent automatically through the CRM. The "From" field will display the name and email address of the employee. An event for the outgoing email will be added to the contact's card.

To send the email, we will sequentially execute three methods:

1. [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) — retrieve the client's data

2. [user.get](../../../api-reference/user/user-get.md) — retrieve the employee's data

3. [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) — create an activity of type "Email"

## 1\. Retrieve Client Data

We will use the method [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) with the client's identifier. The identifier value can be stored in the variable `contactID`. For example, we will retrieve the contact data with the identifier `1`.

{% include [Example Notes](../../../_includes/examples.md) %}

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

{% endlist %}

As a result, we will obtain the client's data, including the email address `EMAIL` and the identifier of the responsible employee `ASSIGNED_BY_ID`.

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

## 2\. Retrieve Employee Data

To get the data of the responsible employee, we will use the method [user.get](../../../api-reference/user/user-get.md) with a filter by the employee's identifier. The identifier should take the value from the `ASSIGNED_BY_ID` field of the `resultContact` object.

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

{% endlist %}

We will obtain the employee's data, including the email address `EMAIL`.

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

## 3\. Create an Activity of Type "Email"

We will prepare the variables:

- `contactEmail` — the first element from the `resultContact` contact,

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

{% endlist %}

To add the event and send the email, we will use the method [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md). We need to pass the client's data, the employee's data, and the activity parameters.

- `SUBJECT` — the email subject. We will specify `subject email now`.

- `DESCRIPTION` — the email body. For example, `body email now`.

- `DESCRIPTION_TYPE` — the text type. Possible values: `1`— plain text, `2`— HTML markup, `3`— BB-code. We will set the value to `3`.

- `COMPLETED` — a flag indicating whether the event is completed. We will specify `Y`.

- `DIRECTION` — the direction of the activity. We will pass `2` — outgoing email. A complete list of activity directions can be obtained using the method [crm.enum.activitydirection](../../../api-reference/crm/auxiliary/enum/outdated/crm-enum-activity-direction.md).

- `OWNER_ID` — the contact identifier. We will pass the variable `contactID`.

- `OWNER_TYPE_ID` — [the identifier of the CRM object type](../../../api-reference/crm/data-types.md#object_type). We will pass `3`— contact. A complete list of object types can be obtained using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md).

- `TYPE_ID` — the activity type. We will specify `4` — email. The list of activity types can be obtained using the method [crm.enum.activitytype](../../../api-reference/crm/auxiliary/enum/outdated/crm-enum-activity-type.md).

- `COMMUNICATIONS` — the client's contact details:

    - `VALUE` — the email address, taking the `VALUE` from the `contactEmail` array,

    - `ENTITY_ID` — the client identifier, passing `contactID`,

    - `ENTITY_TYPE_ID` — [the identifier of the object type](../../../api-reference/crm/data-types.md#object_type), passing `3` — contact.

- `START_TIME` and `END_TIME` — the start and end date and time of the activity. We will specify a duration of 1 hour.

- `RESPONSIBLE_ID` — the identifier of the responsible person, passing `staff.ID`.

- `SETTINGS` — additional settings:

    - `MESSAGE_FROM` — the email sender, passing the name `staff.NAME`, last name `staff.LAST_NAME`, and email address `staff.EMAIL` of the employee.

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

{% endlist %}

If the event is successfully created, the method will return its identifier. If you receive an `error`, refer to the documentation for the method [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) for possible error descriptions.

```json
{
    "result": 3165,
}
```

## Complete Code Example

The code in the example combines all steps: retrieves the client and employee data, adds the "Email" activity, and sends the email to the client.

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

{% endlist %}