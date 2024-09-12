# How to Add a Calendar Event for a Client

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

The example creates an activity in the contact's calendar that needs to be completed within an hour.

{% list tabs %}

- JS

    ```js
    var contactID = 1;
    BX24.callMethod(
        'crm.contact.get',
        {
            'id': contactID
        },
        function(resultContact) {
            if (resultContact.error()) {
                console.error(resultContact.error() + ': ' + resultContact.error_description());
            } else {
                var resultActivity = [];
                if (resultContact.data().ASSIGNED_BY_ID && resultContact.data().PHONE) {
                    var contactPhone = resultContact.data().PHONE[0];
                    var staffID = resultContact.data().ASSIGNED_BY_ID;
                    BX24.callMethod(
                        'crm.activity.add',
                        {
                            'fields': {
                                "SUBJECT": "calendar title",
                                "DESCRIPTION": "calendar body",
                                "DESCRIPTION_TYPE": 3, // text, html, bbCode type id in: BX24.callMethod('crm.enum.contenttype');
                                "OWNER_ID": contactID,
                                "OWNER_TYPE_ID": 3, // BX24.callMethod('crm.enum.ownertype');
                                "TYPE_ID": 1, // BX24.callMethod('crm.enum.activitytype');
                                "COMMUNICATIONS": [
                                    {
                                        'VALUE': contactPhone.VALUE,
                                        'ENTITY_ID': contactID,
                                        'ENTITY_TYPE_ID': 3 // BX24.callMethod('crm.enum.ownertype');
                                    }
                                ],
                                "START_TIME": new Date().toISOString(),
                                "END_TIME": new Date(new Date().getTime() + 3600 * 1000).toISOString(),
                                "RESPONSIBLE_ID": staffID,
                            }
                        },
                        function(resultActivity) {
                            if (resultActivity.error()) {
                                console.error(resultActivity.error() + ': ' + resultActivity.error_description());
                                console.log(JSON.stringify({ 'message': 'Activity not added: ' + resultActivity.error_description() }));
                            } else {
                                console.log(JSON.stringify({ 'message': 'Activity added' }));
                            }
                        }
                    );
                } else {
                    console.log(JSON.stringify({ 'message': 'Activity not added' }));
                }
            }
        }
    );
    ```

- PHP

    {% note info %}

    To use the PHP examples, set up the *CRest* class and include the **crest.php** file in the files where this class is used. [Learn more](../../../how-to-use-examples.md)

    {% endnote %}

    ```php
    $contactID = 1;
    $resultContact = CRest::call(
        'crm.contact.get',
        [
            'id' => $contactID
        ]
    );
    $resultActivity = [];
    if (!empty($resultContact['result']['ASSIGNED_BY_ID']) && !empty($resultContact['result']['PHONE']))
    {
        $contactPhone = reset($resultContact['result']['PHONE']);
        $staffID = $resultContact['result']['ASSIGNED_BY_ID'];
        $resultActivity = CRest::call(
            'crm.activity.add',
            [
                'fields' => [
                    "SUBJECT" => "calendar title",
                    "DESCRIPTION" => "calendar body",
                    "DESCRIPTION_TYPE" => 3,//text,html,bbCode type id in: CRest::call('crm.enum.contenttype');
                    "OWNER_ID" => $contactID,
                    "OWNER_TYPE_ID" => 3, // CRest::call('crm.enum.ownertype');
                    "TYPE_ID" => 1, // CRest::call('crm.enum.activitytype');
                    "COMMUNICATIONS" => [
                        [
                            'VALUE' => $contactPhone['VALUE'],
                            'ENTITY_ID' => $contactID,
                            'ENTITY_TYPE_ID' => 3// CRest::call('crm.enum.ownertype');
                        ]
                    ],
                    "START_TIME" => date("Y-m-d H:i:s", time()),
                    "END_TIME" => date("Y-m-d H:i:s", time() + 3600),
                    "RESPONSIBLE_ID" => $staffID,
                ]
            ]
        );
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
    ```

{% endlist %}