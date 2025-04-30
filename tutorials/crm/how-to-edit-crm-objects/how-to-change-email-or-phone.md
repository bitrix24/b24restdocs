# How to Change or Delete Phone Numbers and E-Mails

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

Examples of adding, changing, and deleting phone numbers and e-mails using a contact as an example.

## Working with E-Mail

{% list tabs %}

- JS

    ```javascript
    let sEmail1 = Math.floor(Math.random() * (999999999 - 111111111 + 1)) + 111111111 + '@nomail.com';
    let sEmail2 = Math.floor(Math.random() * (999999999 - 111111111 + 1)) + 111111111 + '@nomail.com';
    let arNewEmail = [
        {
            'VALUE': sEmail1,
            'VALUE_TYPE': 'HOME'
        },
        {
            'VALUE': sEmail2,
            'VALUE_TYPE': 'HOME'
        }
    ];

    BX24.callMethod(
        "crm.contact.add",
        {
            fields: {
                'NAME': 'CHANGE EMAIL',
                'EMAIL': arNewEmail
            }
        },
        function(newContact) {
            if (newContact.error()) {
                console.error('error create contact ' + newContact.error_description());
            } else {
                BX24.callMethod(
                    "crm.contact.get",
                    {
                        id: newContact.data().result
                    },
                    function(newContactData) {
                        if (newContactData.data().result.EMAIL[0] && newContactData.data().result.EMAIL[1]) {
                            let arUpdateEmail = [
                                {
                                    'ID': newContactData.data().result.EMAIL[0].ID,
                                    'VALUE': Math.floor(Math.random() * (999999999 - 111111111 + 1)) + 111111111 + '@nomail.com'
                                },
                                {
                                    'ID': newContactData.data().result.EMAIL[1].ID,
                                    'VALUE': ''
                                }
                            ];
                            BX24.callMethod(
                                "crm.contact.update",
                                {
                                    id: newContactData.data().result.ID,
                                    fields: {
                                        'EMAIL': arUpdateEmail
                                    }
                                },
                                function(resultContactChange) {
                                    if (resultContactChange.error()) {
                                        console.error(resultContactChange.error());
                                    } else {
                                        console.dir(resultContactChange.data());
                                    }
                                }
                            );
                        }
                    }
                );
            }
        }
    );
    ```

- PHP

    {% note info %}

    To use the examples in PHP, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [Learn more](../../../how-to-use-examples.md)

    {% endnote %}

    ```php
    <?php
    $sEmail1 = rand(111111111, 999999999) . '@nomail.com';
    $sEmail2 = rand(111111111, 999999999) . '@nomail.com';
    $arNewEmail = [
        [
            'VALUE' => $sEmail1,
            'VALUE_TYPE' => 'HOME'
        ],
        [
            'VALUE' => $sEmail2,
            'VALUE_TYPE' => 'HOME'
        ]
    ];
    //create contact with phone
    $newContact = CRest::call(
        'crm.contact.add',
        [
            'fields' => [
                'NAME' => 'CHANGE EMAIL',
                'EMAIL' => $arNewEmail
            ]
        ]
    );
    if ($newContact['result'] > 0) {
        //get contact with email
        $newContactData = CRest::call(
            'crm.contact.get',
            [
                'id' => $newContact['result']
            ]
        );
        //change 1 email and delete 2 email
        if (!empty($newContactData['result']['EMAIL'][0]) && !empty($newContactData['result']['EMAIL'][1])) {
            $arUpdateEmail = [
                [//change
                'ID' => $newContactData['result']['EMAIL'][0]['ID'],
                'VALUE' => rand(111111111, 999999999) . '@nomail.com'
                ],
                [//delete
                'ID' => $newContactData['result']['EMAIL'][1]['ID'],
                'VALUE' => ''//empty value for delete email
                ],
            ];
            $resultContactChange = CRest::call(
                'crm.contact.update',
                [
                    'id' => $newContactData['result']['ID'],
                    'fields' => [
                        'EMAIL' => $arUpdateEmail
                    ]
                ]
            );
        }
    } else {
        echo 'error create contact ' . $newContact['error_description'];
    }
    ?>
    ```

{% endlist %}

## Working with Phone Numbers

{% list tabs %}

- JS

    ```javascript
    let sPhone1 = Math.floor(Math.random() * (999999999 - 111111111 + 1)) + 111111111;
    let sPhone2 = Math.floor(Math.random() * (999999999 - 111111111 + 1)) + 111111111;
    let arNewPhone = [
        {
            'VALUE': sPhone1,
            'VALUE_TYPE': 'HOME'
        },
        {
            'VALUE': sPhone2,
            'VALUE_TYPE': 'HOME'
        }
    ];

    BX24.callMethod(
        "crm.contact.add",
        {
            fields: {
                'NAME': 'CHANGE PHONE',
                'PHONE': arNewPhone
            }
        },
        function(newContact) {
            if (newContact.error()) {
                console.error('error create contact ' + newContact.error_description());
            } else {
                BX24.callMethod(
                    "crm.contact.get",
                    {
                        id: newContact.data().result
                    },
                    function(newContactData) {
                        if (newContactData.data().result.PHONE[0] && newContactData.data().result.PHONE[1]) {
                            let arUpdatePhone = [
                                {
                                    'ID': newContactData.data().result.PHONE[0].ID,
                                    'VALUE': Math.floor(Math.random() * (999999999 - 111111111 + 1)) + 111111111
                                },
                                {
                                    'ID': newContactData.data().result.PHONE[1].ID,
                                    'VALUE': ''
                                }
                            ];
                            BX24.callMethod(
                                "crm.contact.update",
                                {
                                    id: newContactData.data().result.ID,
                                    fields: {
                                        'PHONE': arUpdatePhone
                                    }
                                },
                                function(resultContactChange) {
                                    if (resultContactChange.error()) {
                                        console.error(resultContactChange.error());
                                    } else {
                                        console.dir(resultContactChange.data());
                                    }
                                }
                            );
                        }
                    }
                );
            }
        }
    );
    ```

- PHP

    {% note info %}

    To use the examples in PHP, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [Learn more](../../../how-to-use-examples.md)

    {% endnote %}

    ```php
    <?php
    $sPhone1 = rand(111111111, 999999999);
    $sPhone2 = rand(111111111, 999999999);
    $arNewPhone = [
        [
            'VALUE' => $sPhone1,
            'VALUE_TYPE' => 'HOME'
        },
        [
            'VALUE' => $sPhone2,
            'VALUE_TYPE' => 'HOME'
        ]
    ];
    //create contact with phone
    $newContact = CRest::call(
        'crm.contact.add',
        [
            'fields' => [
                'NAME' => 'CHANGE PHONE',
                'PHONE' => $arNewPhone
            ]
        ]
    );
    if ($newContact['result'] > 0) {
        //get contact with phone
        $newContactData = CRest::call(
            'crm.contact.get',
            [
                'id' => $newContact['result']
            ]
        );
        //change 1 phone and delete 2 phone
        if (!empty($newContactData['result']['PHONE'][0]) && !empty($newContactData['result']['PHONE'][1])) {
            $arUpdatePhone = [
                [//change
                    'ID' => $newContactData['result']['PHONE'][0]['ID'],
                    'VALUE' => rand(111111111, 999999999)
                ],
                [//delete
                    'ID' => $newContactData['result']['PHONE'][1]['ID'],
                    'VALUE' => ''//empty value for delete phone
                ],
            ];
            $resultContactChange = CRest::call(
                'crm.contact.update',
                [
                    'id' => $newContactData['result']['ID'],
                    'fields' => [
                        'PHONE' => $arUpdatePhone
                    ]
                ]
            );
        }
    } else {
        echo 'error create contact ' . $newContact['error_description'];
    }
    ?>
    ```

{% endlist %}