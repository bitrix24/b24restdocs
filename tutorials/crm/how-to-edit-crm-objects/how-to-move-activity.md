# How to Move an Activity Between Items of the Same Type

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to edit CRM items

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Activities associated with CRM items are stored in the item card timeline. Moving an activity from one item to another may be required when a single lead receives several e-mails or calls that, from a business perspective, belong to different leads. In this case, you can split the original lead into several new ones and move the activities to ensure correct data accounting.

The method moves the activity link rather than copying it. As a result of the scenario, the activity will appear in the timeline of the new lead and disappear from the timeline of the original one.

{% note warning "" %}

An activity can only be moved between items of the same type: the values of `sourceEntityTypeId` and `targetEntityTypeId` must match. If the types are different, the method will return error `SOURCE_AND_TARGET_ENTITY_TYPES_ARE_NOT_EQUAL_ERROR`. To move an activity between objects of different types, such as from a lead to a company, use the [How to Move an Activity from One Object Type to Another](./how-to-move-activity-between-objects.md) scenario.

{% endnote %}

To move an activity, we will sequentially execute three methods:

1. [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) — retrieve the activity ID,

2. [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) — create the item to which the activity will be moved (a lead in this example),

3. [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) — perform the activity move.

## 1. Retrieve the Activity ID {#first}

Use the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method with a filter:

- `OWNER_TYPE_ID` — [object type](../../../api-reference/crm/data-types.md#object_type), specify `1` for a lead,

- `OWNER_ID` — the ID of the item from which the activity will be moved.

In this example, we are moving an activity from lead `1000977`. The lead ID is visible in the address bar of its card, for example `/crm/lead/details/1000977/`, or it can be retrieved using the [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md) method.

Without the `select` parameter, the method returns all activity fields. To reduce the response size, specify only the fields required for the scenario: `ID`, `OWNER_TYPE_ID`, `OWNER_ID`, `SUBJECT`, and `DESCRIPTION`. By field `DESCRIPTION` in the [code example](#example), the required activity is selected.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```JavaScript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: "crm.activity.list",
        params: {
            filter:
            {
                "OWNER_TYPE_ID": 1,
                "OWNER_ID": 1000977
            },
            select: [ "ID", "OWNER_TYPE_ID", "OWNER_ID", "SUBJECT", "DESCRIPTION" ]
        }
    });
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token=os.environ["B24_HOOK_TOKEN"],
        )
    )
    # B24_HOOK_TOKEN = 'user_id/webhook_key'

    result = client.crm.activity.list(
        filter={
            "OWNER_TYPE_ID": 1,
            "OWNER_ID": 1000977,
        },
        select=["ID", "OWNER_TYPE_ID", "OWNER_ID", "SUBJECT", "DESCRIPTION"],
    ).response.result
    ```


- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $logger = new Logger('b24');
    $logger->pushHandler(new StreamHandler('php://stdout'));

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $logger))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    $activities = $serviceBuilder->getCRMScope()->activity()->list(
        [],
        [
            'OWNER_TYPE_ID' => 1,
            'OWNER_ID' => 1000977,
        ],
        [
            'ID', 'OWNER_TYPE_ID', 'OWNER_ID', 'SUBJECT', 'DESCRIPTION'
        ],
        0
    )->getActivities();
    ```
{% endlist %}

As a result, you will retrieve all activities associated with the specified item.

```JSON
{
    "result": [
        {
            "ID": "7685",
            "OWNER_TYPE_ID": "1",
            "OWNER_ID": "1000977",
            "SUBJECT": "for leads",
            "DESCRIPTION": "<div>first email</div>\r\n"
        },
        {
            "ID": "7687",
            "OWNER_TYPE_ID": "1",
            "OWNER_ID": "1000977",
            "SUBJECT": "for leads",
            "DESCRIPTION": "<div>second email</div>\r\n"
        }
    ],
    "total": 2
}
```

Select the required activity from the retrieved list and save its `ID`: `7687`. We will pass this value to the parameter `activityId` in step 3.

## 2. Create a New Item {#second}

To create a new lead to which the e-mail activity will be moved, execute the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method with the following parameters:

- `fields[TITLE]` — the lead name,

- `fields[ASSIGNED_BY_ID]` — the identifier of the user responsible for the new lead,

- `params[REGISTER_SONET_EVENT]` — a parameter for registering notifications; specify `Y` so that system notifications are triggered upon the creation of the new lead.

The method must include all mandatory fields for leads in your Bitrix24; otherwise, the lead will not be created. You can check which fields are mandatory using the [crm.lead.fields](../../../api-reference/crm/leads/crm-lead-fields.md) method, which is called without parameters.

{% list tabs %}

- JS

    ```JavaScript
    const result = await $b24.actions.v2.call.make({
        method: "crm.lead.add",
        params: {
            fields:
            {
                TITLE: "Second lead",
                ASSIGNED_BY_ID: 1,
            },
            params: {
                REGISTER_SONET_EVENT: "Y",
            }
        }
    });
    ```

- Python

    ```python
    result = client.crm.lead.add(
        fields={
            "TITLE": "Second lead",
            "ASSIGNED_BY_ID": 1,
        },
        params={
            "REGISTER_SONET_EVENT": "Y",
        },
    ).response.result
    ```


- PHP

    ```php
    $newLeadId = $serviceBuilder->getCRMScope()->lead()->add(
        [
            'TITLE' => 'Second lead',
            'ASSIGNED_BY_ID' => 1,
        ],
        [
            'REGISTER_SONET_EVENT' => 'Y',
        ]
    )->getId();
    ```
{% endlist %}

As a result, you will receive the ID of the created lead. Pass this value to the `targetEntityId` parameter in step 3.

```JSON
{
    "result": 1000979
}
```

## 3. Moving an Activity Between Items

To move an activity, use the [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) method with the following parameters:

- `activityId` — the activity ID obtained in [Step 1](#first) using the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method,

- `sourceEntityTypeId` — the [object type](../../../api-reference/crm/data-types.md#object_type) ID from which the activity is being moved,

- `sourceEntityId` — the item ID from which the activity is being moved,

- `targetEntityTypeId` — the [object type](../../../api-reference/crm/data-types.md#object_type) ID to which the activity is being moved,

- `targetEntityId` — the item ID to which the activity is being moved, obtained in [Step 2](#second) using the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method.

In this example, both object types are equal to `1` — the activity is moved from one lead to another lead.

{% list tabs %}

- JS

    ```JavaScript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.activity.binding.move',
        params: {
            activityId: 7687,
            sourceEntityTypeId: 1,
            sourceEntityId: 1000977,
            targetEntityTypeId: 1,
            targetEntityId: 1000979
        }
    });
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


- PHP

    ```php
    // crm.activity.binding.move does not have a typed wrapper — calling via core
    $result = $serviceBuilder->core->call(
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

As a result, you will receive `true`, indicating the activity move was successful.

```JSON
{
    "result": true
}
```

## Code Example {#example}

{% list tabs %}

- JS

    ```JavaScript
    import { B24Hook } from '@bitrix24/b24jssdk'
    import { createInterface } from 'node:readline/promises'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    async function call(method, params) {
        const result = await $b24.actions.v2.call.make({ method, params });
        if (!result.isSuccess) {
            throw new Error(result.getErrorMessages().join('; '));
        }
        return result.getData().result;
    }

    // Function to execute all steps
    async function transferActivity(firstLeadId, searchPhrase) {
        // Step 1: Get the list of tasks for the specified lead
        const activities = await call("crm.activity.list", {
            filter: {
                "OWNER_TYPE_ID": 1,
                "OWNER_ID": firstLeadId
            },
            select: [ "ID", "OWNER_TYPE_ID", "OWNER_ID", "SUBJECT", "DESCRIPTION" ]
        });

        const targetActivity = activities.find(activity => activity.DESCRIPTION.includes(searchPhrase));

        if (!targetActivity) {
            console.log(`Task with description containing '${searchPhrase}', not found.`);
            return;
        }

        const activityId = targetActivity.ID;

        // Step 2: Create a new lead
        const newLeadId = await call("crm.lead.add", {
            fields: {
                TITLE: "Second lead",
                ASSIGNED_BY_ID: 1,
            },
            params: {
                REGISTER_SONET_EVENT: "Y",
            }
        });

        // Step 3: Move the task
        await call('crm.activity.binding.move', {
            activityId: activityId,
            sourceEntityTypeId: 1,
            sourceEntityId: firstLeadId,
            targetEntityTypeId: 1,
            targetEntityId: newLeadId
        });

        console.log("Task successfully moved.");
    }

    // Requesting the first lead's ID and search phrase from the user
    const rl = createInterface({ input: process.stdin, output: process.stdout });
    const firstLeadId = await rl.question("Enter the first lead's ID: ");
    const searchPhrase = await rl.question("Enter the phrase to search in the email body: ");
    rl.close();

    // Running the function
    try {
        await transferActivity(firstLeadId, searchPhrase);
    } catch (error) {
        console.error(error.message);
    }
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    def transfer_activity(client, first_lead_id, search_phrase):
        try:
            activities = client.crm.activity.list(
                filter={
                    "OWNER_TYPE_ID": 1,
                    "OWNER_ID": first_lead_id,
                },
                select=["ID", "OWNER_TYPE_ID", "OWNER_ID", "SUBJECT", "DESCRIPTION"],
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
            print(f"Task with description containing '{search_phrase}' not found.")
            return

        activity_id = int(target_activity["ID"])

        try:
            new_lead_id = client.crm.lead.add(
                fields={
                    "TITLE": "Second lead",
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
                print("Task successfully moved.")

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token=os.environ["B24_HOOK_TOKEN"],
        )
    )
    # B24_HOOK_TOKEN = 'user_id/webhook_key'

    first_lead_id = int(input("Enter the first lead's ID: "))
    search_phrase = input("Enter the phrase to search in the email body: ")

    transfer_activity(client, first_lead_id, search_phrase)
    ```


- PHP

    ```php
    <?php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $logger = new Logger('b24');
    $logger->pushHandler(new StreamHandler('php://stdout'));

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $logger))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Function to execute all steps
    function transferActivity($serviceBuilder, $firstLeadId, $searchPhrase) {
        $crm = $serviceBuilder->getCRMScope();

        try {
            // Step 1: Get the list of tasks for the specified lead
            $activities = $crm->activity()->list(
                [],
                [
                    'OWNER_TYPE_ID' => 1,
                    'OWNER_ID' => $firstLeadId,
                ],
                [
                    'ID', 'OWNER_TYPE_ID', 'OWNER_ID', 'SUBJECT', 'DESCRIPTION'
                ],
                0
            )->getActivities();

            $targetActivity = null;

            foreach ($activities as $activity) {
                if (strpos((string)$activity->DESCRIPTION, $searchPhrase) !== false) {
                    $targetActivity = $activity;
                    break;
                }
            }

            if (!$targetActivity) {
                echo "Task with description containing '{$searchPhrase}' not found.";
                return;
            }

            $activityId = $targetActivity->ID;

            // Step 2: Create a new lead
            $newLeadId = $crm->lead()->add(
                [
                    'TITLE' => 'Second lead',
                    'ASSIGNED_BY_ID' => 1,
                ],
                [
                    'REGISTER_SONET_EVENT' => 'Y',
                ]
            )->getId();

            // Step 3: Move the task
            // crm.activity.binding.move does not have a typed wrapper — calling via core
            $serviceBuilder->core->call(
                'crm.activity.binding.move',
                [
                    'activityId' => $activityId,
                    'sourceEntityTypeId' => 1,
                    'sourceEntityId' => $firstLeadId,
                    'targetEntityTypeId' => 1,
                    'targetEntityId' => $newLeadId
                ]
            );

            echo 'Task successfully moved.';
        } catch (\Throwable $e) {
            echo 'Error: ' . $e->getMessage();
        }
    }

    // Requesting the first lead's ID and search phrase from the user
    $firstLeadId = readline("Enter the first lead's ID: ");
    $searchPhrase = readline("Enter the phrase to search in the email body: ");

    // Running the function
    transferActivity($serviceBuilder, $firstLeadId, $searchPhrase);
    ```
{% endlist %}

## Verifying the Result

Open the new lead card — the moved activity will appear in the timeline. The activity will no longer be present in the original lead card: the method moves the link rather than copying it.

You can verify the result via REST using the [crm.activity.binding.list](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-list.md) method. Pass the `activityId` of the moved activity to it — the method will return all activity links. After a successful move, only one link will remain in the response: the object type `1` and the new lead ID.

```JSON
{
    "result": [
        {
            "entityTypeId": 1,
            "entityId": 1000979
        }
    ]
}
```

## Errors and Troubleshooting

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `SOURCE_AND_TARGET_ENTITY_TYPES_ARE_NOT_EQUAL_ERROR` | Values `sourceEntityTypeId` and `targetEntityTypeId` do not match. For objects of different types, the activity is moved according to the [How to move an activity from one object type to another](./how-to-move-activity-between-objects.md) scenario ||
|| `SOURCE_AND_TARGET_ENTITY_ID_ARE_EQUAL_ERROR` | The same item was passed in `sourceEntityId` and `targetEntityId`. Please specify different items ||
|| `BINDING_NOT_FOUND` | The activity is not linked to an item from `sourceEntityTypeId` and `sourceEntityId`. Check which item you are moving the activity from ||
|| `ACTIVITY_IS_ALREADY_BOUND` | The activity is already linked to the item you are moving it to ||
|| `NOT_FOUND` | Activity or CRM item not found. Check `activityId`, `sourceEntityId`, and `targetEntityId` ||
|| `ACCESS_DENIED` | The user does not have permission to modify CRM items ||
|| `100` | Mandatory parameters were not passed. The method requires all five: `activityId`, `sourceEntityTypeId`, `sourceEntityId`, `targetEntityTypeId`, and `targetEntityId` ||
|#

## Key Considerations

- The [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) method only moves an activity between items of the same type.
- This scenario does not work only for leads. To move an activity between two deals, specify `2` in `OWNER_TYPE_ID` of Step 1 and in both object type parameters of Step 3, and create the target deal using the [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) method. Values for other types can be found in the [object types](../../../api-reference/crm/data-types.md#object_type) reference.
- The activity is moved along with its entire history: an e-mail or call will disappear from the original item's timeline.
- Along with the link, the activity's own `OWNER_TYPE_ID` and `OWNER_ID` fields change — when only one link remains, its item becomes the owner. Therefore, [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) with a filter for the original lead will no longer return the moved activity; search for it using the new lead instead.
- The [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method requires all mandatory lead fields for your Bitrix24; verify the field composition using the [crm.lead.fields](../../../api-reference/crm/leads/crm-lead-fields.md) method.
- Rerunning the example creates another lead. The already moved activity will not be found in the original lead, and the example will terminate with a message stating the activity was not found.

## Continue Learning

- [{#T}](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md)
- [{#T}](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-list.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)
- [{#T}](./how-to-move-activity-between-objects.md)
- [{#T}](./how-to-change-date-in-activity.md)
