# How to Transfer an Activity Between Entities of the Same Type

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify CRM entities

Activities related to CRM entities are stored in the timeline of the entity's detail form. Transferring an activity from one entity to another may be necessary when multiple emails or calls related to different leads are captured in a single lead. In this case, the original lead can be split into several new ones, and the activities can be transferred for accurate data tracking.

To transfer an activity, we will sequentially execute three methods:

1. [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) — retrieve the activity ID

2. [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) — create an entity to which we will transfer the activity, in this example, a lead

3. [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) — perform the activity transfer

## 1. Retrieve the Activity ID {#first}

We will use the method [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) with the following filters:

- `OWNER_TYPE_ID` — [object type](../../../api-reference/crm/data-types.md#object_type), specify `1` for leads

- `OWNER_ID` — ID of the entity from which we will transfer the activity

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```JavaScript
    BX24.callMethod(
        "crm.activity.list",
        {
            filter:
            {
                "OWNER_TYPE_ID": 1, 
                "OWNER_ID": 1000977 
            },
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.list',
        [
            'filter' => [
                'OWNER_TYPE_ID' => 1, 
                'OWNER_ID' => 1000977 
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive all activities associated with the specified entity.

```JSON
{
    "result": [
        {
            "ID": "7685",
            "OWNER_ID": "1000977",
            "OWNER_TYPE_ID": "1",
            "TYPE_ID": "4",
            "PROVIDER_ID": "CRM_EMAIL",
            "PROVIDER_TYPE_ID": "EMAIL",
            "PROVIDER_GROUP_ID": null,
            "ASSOCIATED_ENTITY_ID": "0",
            "SUBJECT": "for leads",
            "CREATED": "2025-03-10T10:57:41+02:00",
            "LAST_UPDATED": "2025-03-10T10:57:41+02:00",
            "START_TIME": "2025-03-10T10:57:34+02:00",
            "END_TIME": "2025-03-10T20:00:00+02:00",
            "DEADLINE": "9999-12-31T00:00:00+02:00",
            "COMPLETED": "N",
            "STATUS": "1",
            "RESPONSIBLE_ID": "29",
            "PRIORITY": "2",
            "NOTIFY_TYPE": "0",
            "NOTIFY_VALUE": "0",
            "DESCRIPTION": "<div>first email</div>\r\n",
            "DESCRIPTION_TYPE": "3",
            "DIRECTION": "1",
            "LOCATION": "",
            "SETTINGS": {
                "EMAIL_META": {
                    "__email": "some_email@gmail.com",
                    "from": "Some client <some_client@gmail.com>",
                    "replyTo": "",
                    "to": "\"some_email@gmail.com\" <some_email@gmail.com>",
                    "cc": "",
                    "bcc": ""
                },
                "SANITIZE_ON_VIEW": 1
            },
            "ORIGINATOR_ID": null,
            "ORIGIN_ID": null,
            "AUTHOR_ID": "1",
            "EDITOR_ID": "29",
            "PROVIDER_PARAMS": [],
            "PROVIDER_DATA": null,
            "RESULT_MARK": "0",
            "RESULT_VALUE": null,
            "RESULT_SUM": null,
            "RESULT_CURRENCY_ID": null,
            "RESULT_STATUS": "0",
            "RESULT_STREAM": "0",
            "RESULT_SOURCE_ID": null,
            "AUTOCOMPLETE_RULE": "0"
        },
        {
            "ID": "7687",
            "OWNER_ID": "1000977",
            "OWNER_TYPE_ID": "1",
            "TYPE_ID": "4",
            "PROVIDER_ID": "CRM_EMAIL",
            "PROVIDER_TYPE_ID": "EMAIL",
            "PROVIDER_GROUP_ID": null,
            "ASSOCIATED_ENTITY_ID": "0",
            "SUBJECT": "for leads",
            "CREATED": "2025-03-10T10:58:13+02:00",
            "LAST_UPDATED": "2025-03-10T10:58:13+02:00",
            "START_TIME": "2025-03-10T10:58:08+02:00",
            "END_TIME": "2025-03-10T20:00:00+02:00",
            "DEADLINE": "9999-12-31T00:00:00+02:00",
            "COMPLETED": "N",
            "STATUS": "1",
            "RESPONSIBLE_ID": "29",
            "PRIORITY": "2",
            "NOTIFY_TYPE": "0",
            "NOTIFY_VALUE": "0",
            "DESCRIPTION": "<div>second email</div>\r\n",
            "DESCRIPTION_TYPE": "3",
            "DIRECTION": "1",
            "LOCATION": "",
            "SETTINGS": {
                "EMAIL_META": {
                    "__email": "some_email@gmail.com",
                    "from": "Some client <some_client@gmail.com>",
                    "replyTo": "",
                    "to": "\"some_email@gmail.com\" <some_email@gmail.com>",
                    "cc": "",
                    "bcc": ""
                },
                "SANITIZE_ON_VIEW": 1
            },
            "ORIGINATOR_ID": null,
            "ORIGIN_ID": null,
            "AUTHOR_ID": "1",
            "EDITOR_ID": "29",
            "PROVIDER_PARAMS": [],
            "PROVIDER_DATA": null,
            "RESULT_MARK": "0",
            "RESULT_VALUE": null,
            "RESULT_SUM": null,
            "RESULT_CURRENCY_ID": null,
            "RESULT_STATUS": "0",
            "RESULT_STREAM": "0",
            "RESULT_SOURCE_ID": null,
            "AUTOCOMPLETE_RULE": "0"
        }
    ],
    "total": 2
}
```

We will select the desired activity from the list received and save its `ID`: `7687`. In the [code example](#example), the selection of the activity is implemented by searching for a phrase from the `DESCRIPTION` field.

## 2. Create a New Entity {#second}

To create a new lead to which we will transfer the email activity, we will execute the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) with the following parameters:

- `fields[TITLE]` — title of the lead

- `fields[ASSIGNED_BY_ID]` — identifier of the person responsible for the new lead

- `params[REGISTER_SONET_EVENT]` — parameter for registering notifications, specify `Y` so that system notifications are triggered for the new lead upon creation

All required fields for leads in your Bitrix24 must be specified in the method; otherwise, the lead will not be created. You can check which fields are required using the method [crm.lead.fields](../../../api-reference/crm/leads/crm-lead-fields.md), which is called without parameters.

{% list tabs %}

- JS

    ```JavaScript
    BX24.callMethod(
        "crm.lead.add",
        {
            fields:
            {
                TITLE: "Second lead", 
                ASSIGNED_BY_ID: 1, 
            },
            params: {
                REGISTER_SONET_EVENT: "Y", 
            }
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.lead.add',
        [
            'fields' => [
                'TITLE' => 'Second lead', 
                'ASSIGNED_BY_ID' => 1, 
            ],
            'params' => [
                'REGISTER_SONET_EVENT' => 'Y', 
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive the ID of the created lead.

```JSON
{
    "result": 1000979
}
```

## 3. Transfer the Activity Between Entities

To transfer the activity, we will use the method [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) with the following parameters:

- `activityId` — ID of the activity, obtained in [step 1](#first) using the method [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)

- `sourceEntityTypeId` — ID of the [object type](../../../api-reference/crm/data-types.md#object_type) from which we are transferring the activity

- `sourceEntityId` — ID of the entity from which we are transferring the activity

- `targetEntityTypeId` — ID of the [object type](../../../api-reference/crm/data-types.md#object_type) to which we are transferring the activity

- `targetEntityId` — ID of the entity to which we are transferring the activity, obtained in [step 2](#second) using the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md)

`sourceEntityTypeId` and `targetEntityTypeId` must have the same object type value.

{% list tabs %}

- JS

    ```JavaScript
    BX24.callMethod(
        'crm.activity.binding.move',
        {
            activityId: 7687, 
            sourceEntityTypeId: 1, 
            sourceEntityId: 1000977, 
            targetEntityTypeId: 1, 
            targetEntityId: 1000979 
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.binding.move',
        [
            'activityId' => 7687, 
            'sourceEntityTypeId' => 1, 
            'sourceEntityId' => 1000977, 
            'targetEntityTypeId' => 1,
            'targetEntityId' => 1000979 
        ]
    );
    ```

{% endlist %}

As a result, we will receive `true`, indicating that the activity transfer was successful. If you receive an `error` in the result, review the possible error descriptions in the documentation for the method [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md#error-handling).

```JSON
{
    "result": true
}
```

## Code Example {#example}

{% list tabs %}

- JS

    ```JavaScript
    // Function to execute all steps
    function transferActivity() {
        // Prompt user for the ID of the first lead
        const firstLeadId = prompt("Enter the ID of the first lead:");

        // Prompt user for the search phrase for the description field
        const searchPhrase = prompt("Enter the phrase to search in the email body:");

        // Step 1: Retrieve the list of activities for the specified lead
        BX24.callMethod(
            "crm.activity.list",
            {
                filter: {
                    "OWNER_TYPE_ID": 1,
                    "OWNER_ID": firstLeadId
                },
            },
            function(result) {
                if (result.error()) {
                    console.error(result.error());
                    return;
                }

                const activities = result.data();
                const targetActivity = activities.find(activity => activity.DESCRIPTION.includes(searchPhrase));

                if (!targetActivity) {
                    console.log(`Activity with description containing '${searchPhrase}' not found.`);
                    return;
                }

                const activityId = targetActivity.ID;

                // Step 2: Create a new lead
                BX24.callMethod(
                    "crm.lead.add",
                    {
                        fields: {
                            TITLE: "Second lead",
                            ASSIGNED_BY_ID: 1,
                        },
                        params: {
                            REGISTER_SONET_EVENT: "Y",
                        }
                    },
                    function(result) {
                        if (result.error()) {
                            console.error(result.error());
                            return;
                        }

                        const newLeadId = result.data();

                        // Step 3: Transfer the activity
                        BX24.callMethod(
                            'crm.activity.binding.move',
                            {
                                activityId: activityId,
                                sourceEntityTypeId: 1,
                                sourceEntityId: firstLeadId,
                                targetEntityTypeId: 1,
                                targetEntityId: newLeadId
                            },
                            function(result) {
                                if (result.error()) {
                                    console.error(result.error());
                                } else {
                                    console.log("Activity successfully transferred.");
                                }
                            }
                        );
                    }
                );
            }
        );
    }

    // Execute the function
    transferActivity();
    ```

- PHP

    ```php
    require_once('crest.php');

    // Function to execute all steps
    function transferActivity($firstLeadId, $searchPhrase) {
        // Step 1: Retrieve the list of activities for the specified lead
        $result = CRest::call(
            'crm.activity.list',
            [
                'filter' => [
                    'OWNER_TYPE_ID' => 1,
                    'OWNER_ID' => $firstLeadId
                ]
            ]
        );

        if (isset($result['error'])) {
            echo 'Error: ' . $result['error_description'];
            return;
        }

        $activities = $result['result'];
        $targetActivity = null;

        foreach ($activities as $activity) {
            if (strpos($activity['DESCRIPTION'], $searchPhrase) !== false) {
                $targetActivity = $activity;
                break;
            }
        }

        if (!$targetActivity) {
            echo "Activity with description containing '{$searchPhrase}' not found.";
            return;
        }

        $activityId = $targetActivity['ID'];

        // Step 2: Create a new lead
        $leadResult = CRest::call(
            'crm.lead.add',
            [
                'fields' => [
                    'TITLE' => 'Second lead',
                    'ASSIGNED_BY_ID' => 1,
                ],
                'params' => [
                    'REGISTER_SONET_EVENT' => 'Y',
                ]
            ]
        );

        if (isset($leadResult['error'])) {
            echo 'Error: ' . $leadResult['error_description'];
            return;
        }

        $newLeadId = $leadResult['result'];

        // Step 3: Transfer the activity
        $moveResult = CRest::call(
            'crm.activity.binding.move',
            [
                'activityId' => $activityId,
                'sourceEntityTypeId' => 1,
                'sourceEntityId' => $firstLeadId,
                'targetEntityTypeId' => 1,
                'targetEntityId' => $newLeadId
            ]
        );

        if (isset($moveResult['error'])) {
            echo 'Error: ' . $moveResult['error_description'];
        } else {
            echo 'Activity successfully transferred.';
        }
    }

    // Prompt user for the ID of the first lead and the search phrase
    $firstLeadId = readline("Enter the ID of the first lead: ");
    $searchPhrase = readline("Enter the phrase to search in the email body: ");

    // Execute the function
    transferActivity($firstLeadId, $searchPhrase);
    ```

{% endlist %}