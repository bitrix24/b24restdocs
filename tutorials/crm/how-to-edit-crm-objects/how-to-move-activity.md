# How to Transfer a Deal Between Entities of the Same Type

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify CRM entities

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Deals associated with CRM entities are stored in the timeline of the entity's detail form. Transferring a deal from one entity to another may be necessary when multiple emails or calls related to different leads are captured under a single lead. In this case, the original lead can be split into several new ones, and the deals can be transferred for accurate data tracking.

To transfer a deal, we will sequentially execute three methods:

1. [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) — retrieve the activity ID

2. [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) — create the item to which the activity will be moved (in this example, a lead)

3. [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) — perform the activity transfer

## 1. Retrieving the Deal ID {#first}

We will use the method [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) with the following filter:

- `OWNER_TYPE_ID` — [object type](../../../api-reference/crm/data-types.md#object_type), specify `1` for a lead

- `OWNER_ID` — the ID of the item from which the activity will be moved

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
        }
    });
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
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $serviceBuilder->getCRMScope()->activity()->list(
        [],
        [
            'OWNER_TYPE_ID' => 1,
            'OWNER_ID' => 1000977,
        ],
        [],
        0
    );
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

    result = client.crm.activity.list(
        filter={
            "OWNER_TYPE_ID": 1,
            "OWNER_ID": 1000977,
        }
    ).response.result
    ```

{% endlist %}

As a result, you will retrieve all activities associated with the specified item.

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
    "total": 2,
}
```

Select the required activity from the retrieved list and save its `ID`: `7687`. In [the code example](#example), task selection is implemented via phrase search from the field `DESCRIPTION`.

## 2. Creating a New Entity {#second}

To create a new lead to which the e-mail activity will be moved, call the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method with the following parameters:

- `fields[TITLE]` — the lead name

- `fields[ASSIGNED_BY_ID]` — the identifier of the user responsible for the new lead

- `params[REGISTER_SONET_EVENT]` — a parameter for registering notifications; specify `Y` so that system notifications are triggered upon the creation of the new lead

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

- PHP

    ```php
    $result = $serviceBuilder->getCRMScope()->lead()->add(
        [
            'TITLE' => 'Second lead',
            'ASSIGNED_BY_ID' => 1,
        ],
        [
            'REGISTER_SONET_EVENT' => 'Y',
        ]
    );
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

{% endlist %}

As a result, you will receive the ID of the created lead.

```JSON
{
    "result": 1000979,
}
```

## 3. Transferring the Deal Between Entities

To move the activity, use the [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) method with the following parameters:

- `activityId` — the activity ID obtained in [step 1](#first) via the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method

- `sourceEntityTypeId` — the ID of the [object type](../../../api-reference/crm/data-types.md#object_type) from which the activity is being moved

- `sourceEntityId` — the ID of the item from which the activity is being moved

- `targetEntityTypeId` — the ID of the [object type](../../../api-reference/crm/data-types.md#object_type) to which the activity is being moved

- `targetEntityId` — the ID of the item to which the activity is being moved, obtained in [step 2](#second) via the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method

`sourceEntityTypeId` and `targetEntityTypeId` must have the same object type value.

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

- PHP

    ```php
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

As a result, you will receive `true`, the task transfer was successful. If you received an error as a result `error`, study the description of possible errors in the [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md#error-handling) method documentation.

```JSON
{
    "result": true,
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

    // Function to perform all steps
    async function transferActivity(firstLeadId, searchPhrase) {
        // Step 1: Get the task list for the specified lead
        const activities = await call("crm.activity.list", {
            filter: {
                "OWNER_TYPE_ID": 1,
                "OWNER_ID": firstLeadId
            }
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

        // Step 3: Transfer the task
        await call('crm.activity.binding.move', {
            activityId: activityId,
            sourceEntityTypeId: 1,
            sourceEntityId: firstLeadId,
            targetEntityTypeId: 1,
            targetEntityId: newLeadId
        });

        console.log("Task successfully transferred.");
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
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Function to perform all steps
    function transferActivity($serviceBuilder, $firstLeadId, $searchPhrase) {
        $crm = $serviceBuilder->getCRMScope();

        try {
            // Step 1: Get the task list for the specified lead
            $activities = $crm->activity()->list(
                [],
                [
                    'OWNER_TYPE_ID' => 1,
                    'OWNER_ID' => $firstLeadId,
                ],
                [],
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

            // Step 3: Transfer the task
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

            echo 'Task successfully transferred.';
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
                print("Task successfully transferred.")

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    first_lead_id = int(input("Enter the first lead's ID: "))
    search_phrase = input("Enter the phrase to search in the email body: ")

    transfer_activity(client, first_lead_id, search_phrase)
    ```

{% endlist %}
