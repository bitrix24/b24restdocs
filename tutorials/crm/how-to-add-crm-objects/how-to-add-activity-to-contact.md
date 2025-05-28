# Add Calendar Event for Client Interaction

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify CRM entity

Calendar events can be added automatically to remind employees about meetings or calls with clients. An event linked to the client's contact will appear in the calendar of the responsible employee. A task will be added to the contact's detail form for the event.

To add an event to the calendar, we will sequentially execute two methods:

1. [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) — retrieve client data

2. [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) — create a calendar event

## 1. Retrieve Client Data

We will use the method [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) with the client ID. For example, we are interested in the contact with ID `1`.

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

-  JS

   ```javascript
   BX24.callMethod(
       'crm.contact.get',
       {
           'id': 1
       },
   );
   ```

-  PHP

   ```php
   require_once('crest.php');
   
   $resultContact = CRest::call(
       'crm.contact.get',
       [
           'id' => 1
       ]
   );
   ```

{% endlist %}

As a result, we will receive client data, including phone `PHONE` and the ID of the responsible employee `ASSIGNED_BY_ID`.

```json
{
    "result": {
        "ID": "1",
        "POST": "CEO",
        "COMMENTS": null,
        "NAME": "Alex",
        "SECOND_NAME": "Kirillovich",
        "LAST_NAME": "Vronsky",
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
        "DATE_CREATE": "2023-08-18T12:43:42+02:00",
        "DATE_MODIFY": "2023-10-17T15:59:13+02:00",
        "ASSIGNED_BY_ID": "61",
        "CREATED_BY_ID": "57",
        "MODIFY_BY_ID": "47",
        "OPENED": "N",
        "ORIGINATOR_ID": null,
        "ORIGIN_ID": null,
        "ORIGIN_VERSION": null,
        "FACE_ID": null,
        "LAST_ACTIVITY_TIME": "2025-03-15T10:38:21+01:00",
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
            "VALUE": "88001001020",
            "TYPE_ID": "PHONE"
        },
        ],
        "EMAIL": [
        {
            "ID": "1328",
            "VALUE_TYPE": "WORK",
            "VALUE": "vronsky@example.com",
            "TYPE_ID": "EMAIL"
        },
        ]
    },
    "time": {
        "start": 1747737934.888428,
        "finish": 1747737934.945823,
        "duration": 0.057394981384277344,
        "processing": 0.029510021209716797,
        "date_start": "2025-05-20T13:45:34+02:00",
        "date_finish": "2025-05-20T13:45:34+02:00"
    }
}
```

## 2. Create Calendar Event

To create an event, we will use the method [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md). We need to pass the client data and arbitrary parameters for the new event.

- `SUBJECT` — event title. We will specify `calendar title`.

- `DESCRIPTION` — description. For example, `calendar body`.

- `DESCRIPTION_TYPE` — format of the description text. Possible values: `1` — plain text, `2` — HTML markup, `3` — BB-code. We will set the value to `3`.

- `OWNER_ID` — contact ID. We will pass the client ID — `1`.

- `OWNER_TYPE_ID` — [CRM object type ID](../../../api-reference/crm/data-types.md#object_type). We will pass `3` — contact. A complete list of object types can be obtained using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md).

- `TYPE_ID` — event type. We will specify `1` — meeting. The list of event types can be obtained using the method [crm.enum.activitytype](../../../api-reference/crm/auxiliary/enum/crm-enum-activity-type.md).

- `COMMUNICATIONS` — client's contact details:

    - `VALUE` — phone number, we will take the `VALUE` from the `PHONE` array obtained in the first step,

    - `ENTITY_ID` — client ID, we will pass `1`,

    - `ENTITY_TYPE_ID` — [object type ID](../../../api-reference/crm/data-types.md#object_type), we will pass `3` — contact.

- `START_TIME` and `END_TIME` — start and end date and time in [ISO 8601](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) format, we will specify, for example, a duration of one hour,

- `RESPONSIBLE_ID` — ID of the responsible person, we will pass `ASSIGNED_BY_ID`, which was obtained in the first step.

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'crm.activity.add',
        {
            'fields': {
                "SUBJECT": "calendar title",
                "DESCRIPTION": "calendar body",
                "DESCRIPTION_TYPE": 3,
                "OWNER_ID": 1, 
                "OWNER_TYPE_ID": 3, 
                "TYPE_ID": 1, 
                "COMMUNICATIONS": [
                    {
                        'VALUE': "88001001020", 
                        'ENTITY_ID': 1, 
                        'ENTITY_TYPE_ID': 3
                    }
                ],
                "START_TIME": "2025-05-20T14:00:00",
                "END_TIME": "2025-05-20T15:00:00",
                "RESPONSIBLE_ID": 61 
            }
        },
    );
    ```

-  PHP

    ```php
    require_once('crest.php');
    
    $result = CRest::call(
            'crm.activity.add',
            [
                'fields' => [
                    "SUBJECT" => "calendar title",
                    "DESCRIPTION" => "calendar body",
                    "DESCRIPTION_TYPE" => 3,
                    "OWNER_ID" => 1,
                    "OWNER_TYPE_ID" => 3,
                    "TYPE_ID" => 1,
                    "COMMUNICATIONS" => [
                        [
                            'VALUE' => "88001001020",
                            'ENTITY_ID' => 1,
                            'ENTITY_TYPE_ID' => 3
                        ]
                    ],
                    "START_TIME" => "2025-05-20T14:00:00",
                    "END_TIME" => "2025-05-20T15:00:00",
                    "RESPONSIBLE_ID" => 61,
                ]
            ]
        );
    ```

{% endlist %}

If the event is created successfully, the method will return its ID. If you receive an `error`, review the possible error descriptions in the documentation for the method [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md).

```json
{
    "result": 6915,
}
```

## Code Example

The example creates a task "Meeting" in the CRM contact detail form and an event lasting one hour in the employee's calendar.

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