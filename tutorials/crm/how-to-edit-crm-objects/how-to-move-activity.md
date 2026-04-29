# How to Transfer a Deal Between Entities of the Same Type

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify CRM entities

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Deals associated with CRM entities are stored in the timeline of the entity's detail form. Transferring a deal from one entity to another may be necessary when multiple emails or calls related to different leads are captured under a single lead. In this case, the original lead can be split into several new ones, and the deals can be transferred for accurate data tracking.

To transfer a deal, we will sequentially execute three methods:

1. [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) — retrieve the deal ID

2. [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) — create the entity to which we will transfer the deal, in this example, a lead

3. [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) — perform the deal transfer

## 1. Retrieving the Deal ID {#first}

We will use the method [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) with the following filter:

- `OWNER_TYPE_ID` — [object type](../../../api-reference/crm/data-types.md#object_type), specify `1` for leads

- `OWNER_ID` — ID of the entity from which we will transfer the deal

{% include [Example Notes](../../../_includes/examples.md) %}

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

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            auth_token="your-webhook-token",
        )
    )

    result = client.crm.activity.list(
        filter={
            "OWNER_TYPE_ID": 1,
            "OWNER_ID": 1000977,
        }
    ).response.result
    ```

{% endlist %}

As a result, we will receive all deals associated with the specified entity.

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
            "CREATED": "2025-03-10T10:57:41+03:00",
            "LAST_UPDATED": "2025-03-10T10:57:41+03:00",
            "START_TIME": "2025-03-10T10:57:34+03:00",
            "END_TIME": "2025-03-10T20:00:00+03:00",
            "DEADLINE": "9999-12-31T00:00:00+03:00",
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
            "CREATED": "2025-03-10T10:58:13+03:00",
            "LAST_UPDATED": "2025-03-10T10:58:13+03:00",
            "START_TIME": "2025-03-10T10:58:08+03:00",
            "END_TIME": "2025-03-10T20:00:00+03:00",
            "DEADLINE": "9999-12-31T00:00:00+03:00",
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

Select the desired deal from the list obtained and save its `ID`: `7687`. In the [code example](#example), the deal selection is implemented through a search by the phrase from the `DESCRIPTION` field.

## 2. Creating a New Entity {#second}

To create a new lead to which we will transfer the email deal, we will execute the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) with the following parameters:

- `fields[TITLE]` — title of the lead

- `fields[ASSIGNED_BY_ID]` — identifier of the person responsible for the new lead

- `params[REGISTER_SONET_EVENT]` — parameter for registering notifications, specify `Y` to trigger system notifications for the new lead upon creation

All required fields for leads in your Bitrix24 must be specified in the method; otherwise, the lead will not be created. You can check which fields are mandatory using the method [crm.lead.fields](../../../api-reference/crm/leads/crm-lead-fields.md), which is called without parameters.

{% list tabs %}

- JS

    ```JavaScript
    BX24.callMethod(
        "crm.lead.add",
        {
            fields:
            {
                TITLE: "Second Lead", 
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
                'TITLE' => 'Second Lead', 
                'ASSIGNED_BY_ID' => 1, 
            ],
            'params' => [
                'REGISTER_SONET_EVENT' => 'Y', 
            ]
        ]
    );
    ```

- Python

    ```python
    result = client.crm.lead.add(
        fields={
            "TITLE": "Second Lead",
            "ASSIGNED_BY_ID": 1,
        },
        params={
            "REGISTER_SONET_EVENT": "Y",
        },
    ).response.result
    ```

{% endlist %}

As a result, we will receive the ID of the created lead.

```JSON
{
    "result": 1000979
}
```

## 3. Transferring the Deal Between Entities

To transfer the deal, we will use the method [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) with the following parameters:

- `activityId` — ID of the deal obtained in [step 1](#first) using the method [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)

- `sourceEntityTypeId` — ID of the [object type](../../../api-reference/crm/data-types.md#object_type) from which we are transferring the deal

- `sourceEntityId` — ID of the entity from which we are transferring the deal

- `targetEntityTypeId` — ID of the [object type](../../../api-reference/crm/data-types.md#object_type) to which we are transferring the deal

- `targetEntityId` — ID of the entity to which we are transferring the deal, obtained in [step 2](#second) using the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md)

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

- Python

    ```python
    result = client.crm.activity.binding.move(
        activity_id=7687,
        source_entity_type_id=1,
        source_entity_id=1000977,
        target_entity_type_id=1,
        target_entity_id=1000979,
    ).response.result
    ```

{% endlist %}

As a result, we will receive `true`, indicating that the deal transfer was successful. If you receive an `error` in the result, refer to the documentation for the method [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md#error-handling) to understand possible errors.

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

        // Prompt user for the phrase to search in the email body
        const searchPhrase = prompt("Enter the phrase to search in the email body:");

        // Step 1: Retrieve the list of deals for the specified lead
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
                    console.log(`No deal found with description containing '${searchPhrase}'.`);
                    return;
                }

                const activityId = targetActivity.ID;

                // Step 2: Create a new lead
                BX24.callMethod(
                    "crm.lead.add",
                    {
                        fields: {
                            TITLE: "Second Lead",
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

                        // Step 3: Transfer the deal
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
                                    console.log("Deal successfully transferred.");
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
        // Step 1: Retrieve the list of deals for the specified lead
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
            echo "No deal found with description containing '{$searchPhrase}'.";
            return;
        }

        $activityId = $targetActivity['ID'];

        // Step 2: Create a new lead
        $leadResult = CRest::call(
            'crm.lead.add',
            [
                'fields' => [
                    'TITLE' => 'Second Lead',
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

        // Step 3: Transfer the deal
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
            echo 'Deal successfully transferred.';
        }
    }

    // Prompt user for the ID of the first lead and the search phrase
    $firstLeadId = readline("Enter the ID of the first lead: ");
    $searchPhrase = readline("Enter the phrase to search in the email body: ");

    // Execute the function
    transferActivity($firstLeadId, $searchPhrase);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError


    def transfer_activity(client, first_lead_id, search_phrase):
        try:
            activities = client.crm.activity.list(
                filter={
                    "OWNER_TYPE_ID": 1,
                    "OWNER_ID": first_lead_id,
                }
            ).response.result
        except BitrixAPIError as error:
            print(f"Error: {error}")
            return

        target_activity = None
        for activity in activities:
            if search_phrase in str(activity.get("DESCRIPTION") or ""):
                target_activity = activity
                break

        if target_activity is None:
            print(f"No activity found with description containing '{search_phrase}'.")
            return

        activity_id = int(target_activity["ID"])

        try:
            new_lead_id = client.crm.lead.add(
                fields={
                    "TITLE": "Second Lead",
                    "ASSIGNED_BY_ID": 1,
                },
                params={
                    "REGISTER_SONET_EVENT": "Y",
                },
            ).response.result
        except BitrixAPIError as error:
            print(f"Error: {error}")
            return

        try:
            result = client.crm.activity.binding.move(
                activity_id=activity_id,
                source_entity_type_id=1,
                source_entity_id=first_lead_id,
                target_entity_type_id=1,
                target_entity_id=new_lead_id,
            ).response.result
        except BitrixAPIError as error:
            print(f"Error: {error}")
        else:
            if result:
                print("Deal successfully transferred.")


    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            auth_token="your-webhook-token",
        )
    )

    first_lead_id = int(input("Enter the ID of the first lead: "))
    search_phrase = input("Enter the phrase to search in the email body: ")

    transfer_activity(client, first_lead_id, search_phrase)
    ```

{% endlist %}