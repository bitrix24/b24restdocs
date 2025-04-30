# How to Send an E-mail to a Client on Behalf of an Employee Indicating the Employee's E-mail

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

The example retrieves the e-mail of the person responsible for the contact and creates an activity for sending the e-mail with immediate dispatch.

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

    {% note info %}

    To use the examples in PHP, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [More details](../../../how-to-use-examples.md)

    {% endnote %}

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