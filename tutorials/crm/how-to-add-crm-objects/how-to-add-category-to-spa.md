# How to Create a New Sales Funnel with Stages in a Smart Process

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute methods: users with administrative access to the CRM section

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

Sales funnels allow you to divide work in the CRM into different stages. For example, a sale may consist of three funnels: sales, delivery, and post-sale service. Access permissions and the view of the CRM object card can be configured for each funnel.

To create a new funnel in a smart process, we will sequentially execute the following methods:

1. [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) — retrieve the numeric identifier of the smart process type.
2. [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) — create a new funnel.
3. [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — retrieve the pre-installed stages of the new funnel.
4. [crm.status.update](../../../api-reference/crm/status/crm-status-update.md) — modify a pre-installed stage.
5. [crm.status.add](../../../api-reference/crm/status/crm-status-add.md) — create a new stage.

## 1. Retrieve the Smart Process Identifier

We will use the method [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) with a filter by the smart process name `title`.

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: 'crm.type.list',
        params: {
            filter: { title: 'Equipment procurement' }
        }
    });

    if (!result.isSuccess) {
        console.error(result.getErrorMessages().join('; '));
    } else {
        const types = result.getData().result.types;
        if (types.length > 0) {
            var entityTypeId = types[0].entityTypeId;
            console.log('entityTypeId:', entityTypeId);
        }
    }
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

    $types = $serviceBuilder->getCRMScope()->type()
        ->list([], ['title' => 'Equipment procurement'])
        ->getTypes();
    $entityTypeId = $types[0]->entityTypeId;
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

    entity_type_id = int(
        client.crm.type.list(
            filter={"title": "Equipment procurement"},
        ).response.result["types"][0]["entityTypeId"]
    )
    ```

{% endlist %}

As a result, you will retrieve and save the `entityTypeId` of the required SPA.

```json
{
    "result": {
        "types": [
            {
                "id": 7,
                "title": "Equipment procurement",
                "code": "",
                "createdBy": 1,
                "entityTypeId": 177,
                "customSectionId": null,
                "isCategoriesEnabled": "Y",
                "isStagesEnabled": "Y",
                "isBeginCloseDatesEnabled": "Y",
                "isClientEnabled": "Y",
                "isUseInUserfieldEnabled": "Y",
                "isLinkWithProductsEnabled": "Y",
                "isMycompanyEnabled": "Y",
                "isDocumentsEnabled": "Y",
                "isSourceEnabled": "Y",
                "isObserversEnabled": "Y",
                "isRecyclebinEnabled": "Y",
                "isAutomationEnabled": "Y",
                "isBizProcEnabled": "Y",
                "isSetOpenPermissions": "Y",
                "isPaymentsEnabled": "N",
                "isCountersEnabled": "N",
                "createdTime": "2021-11-26T10:52:17+03:00",
                "updatedTime": "2024-11-12T15:32:39+03:00",
                "updatedBy": 1,
                "isInitialized": "Y"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1751955574.022139,
        "finish": 1751955574.065841,
        "duration": 0.043701887130737305,
        "processing": 0.00709080696105957,
        "date_start": "2025-07-08T09:19:34+03:00",
        "date_finish": "2025-07-08T09:19:34+03:00",
        "operating_reset_at": 1751956174,
        "operating": 0
    }
}
```

## 2. Create a New Funnel

We will use the method [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) with the following parameters:

- `entityTypeId` — the numeric type identifier from the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method,
- `fields[name]` — the pipeline name,
- `fields[sort]` — the pipeline sorting. Sorting affects the pipeline's position in the list.

{% list tabs %}

- JS
  
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.category.add',
        params: {
            entityTypeId: entityTypeId,
            fields: {
                name: 'New funnel',
                sort: 100
            }
        }
    });

    if (!result.isSuccess) {
        console.error(result.getErrorMessages().join('; '));
    } else {
        var categoryId = result.getData().result.category.id;
        console.log('categoryId:', categoryId);
    }
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->core->call(
        'crm.category.add',
        [
            'entityTypeId' => $entityTypeId,
            'fields' => [
                'name' => 'New funnel',
                'sort' => 100,
            ]
        ]
    );
    $categoryId = $result->getResponseData()->getResult()['category']['id'];
    ```

- Python

    ```python
    category_id = int(
        client.crm.category.add(
            entity_type_id=entity_type_id,
            fields={
                "name": "New funnel",
                "sort": 100,
            },
        ).response.result["category"]["id"]
    )
    ```

{% endlist %}

As a result, you will retrieve and save the `id` of the created pipeline.

```json
{
    "result": {
        "category": {
            "id": 39,
            "name": "New funnel",
            "sort": 100,
            "entityTypeId": 177,
            "isDefault": "N"
        }
    },
    "time": {
        "start": 1751955674.679973,
        "finish": 1751955674.87359,
        "duration": 0.1936171054840088,
        "processing": 0.1517810821533203,
        "date_start": "2025-07-08T09:21:14+03:00",
        "date_finish": "2025-07-08T09:21:14+03:00",
        "operating_reset_at": 1751956274,
        "operating": 0.15175914764404297
    }
}
```

## 3. Retrieve the Stages of the Created Funnel

We will use the method [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) with the filter:

- `ENTITY_ID` — the CRM directory identifier. For SPA stages, the identifier follows the format `DYNAMIC_{entityTypeId}_STAGE_{categoryId}`:
	- `{entityTypeId}` — the numeric SPA type identifier from the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method,
	- `{categoryId}` — the pipeline identifier from the [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) method.

{% list tabs %}

- JS
  
    ```javascript
    var entityId = `DYNAMIC_${entityTypeId}_STAGE_${categoryId}`;
    const result = await $b24.actions.v2.call.make({
        method: 'crm.status.list',
        params: {
            filter: { ENTITY_ID: entityId }
        }
    });

    if (!result.isSuccess) {
        console.error(result.getErrorMessages().join('; '));
    } else {
        var stages = result.getData().result;
        console.log('Stages:', stages);
    }
    ```
- PHP
  
    ```php
    $entityId = "DYNAMIC_{$entityTypeId}_STAGE_{$categoryId}";
    $stages = $serviceBuilder->getCRMScope()->status()
        ->list([], ['ENTITY_ID' => $entityId])
        ->getStatuses();
    ```

- Python

    ```python
    entity_id = f"DYNAMIC_{entity_type_id}_STAGE_{category_id}"
    stages = client.crm.status.list(
        filter={"ENTITY_ID": entity_id},
    ).response.result
    ```

{% endlist %}

As a result, we will obtain the pre-installed stages of the funnel. By default, each new funnel has five stages:

- three "In Progress" stages `SEMANTICS: ""`,
- one "Success" stage `SEMANTICS: "S"`,
- one "Failure" stage `SEMANTIC": "F"`.
  
Each pipeline must have at least one stage from each group. A pipeline can have only one success stage.

```json
{
    "result": [
        {
            "ID": "737",
            "ENTITY_ID": "DYNAMIC_177_STAGE_39",
            "STATUS_ID": "DT177_39:NEW",
            "NAME": "Start",
            "NAME_INIT": "Start",
            "SORT": "10",
            "SYSTEM": "Y",
            "CATEGORY_ID": "39",
            "COLOR": "#22B9FF",
            "SEMANTICS": null
        },
        {
            "ID": "739",
            "ENTITY_ID": "DYNAMIC_177_STAGE_39",
            "STATUS_ID": "DT177_39:PREPARATION",
            "NAME": "Preparation",
            "NAME_INIT": "Preparation",
            "SORT": "20",
            "SYSTEM": "N",
            "CATEGORY_ID": "39",
            "COLOR": "#88B9FF",
            "SEMANTICS": null
        },
        {
            "ID": "741",
            "ENTITY_ID": "DYNAMIC_177_STAGE_39",
            "STATUS_ID": "DT177_39:CLIENT",
            "NAME": "Approval",
            "NAME_INIT": "Approval",
            "SORT": "30",
            "SYSTEM": "N",
            "CATEGORY_ID": "39",
            "COLOR": "#10e5fc",
            "SEMANTICS": null
        },
        {
            "ID": "743",
            "ENTITY_ID": "DYNAMIC_177_STAGE_39",
            "STATUS_ID": "DT177_39:SUCCESS",
            "NAME": "Success",
            "NAME_INIT": "Success",
            "SORT": "40",
            "SYSTEM": "Y",
            "CATEGORY_ID": "39",
            "COLOR": "#00ff00",
            "SEMANTICS": "S"
        },
        {
            "ID": "745",
            "ENTITY_ID": "DYNAMIC_177_STAGE_39",
            "STATUS_ID": "DT177_39:FAIL",
            "NAME": "Failure",
            "NAME_INIT": "Failure",
            "SORT": "50",
            "SYSTEM": "Y",
            "CATEGORY_ID": "39",
            "COLOR": "#ff0000",
            "SEMANTICS": "F"
        }
    ],
    "total": 5,
    "time": {
        "start": 1751956021.475235,
        "finish": 1751956021.514927,
        "duration": 0.039691925048828125,
        "processing": 0.0024650096893310547,
        "date_start": "2025-07-08T09:27:01+03:00",
        "date_finish": "2025-07-08T09:27:01+03:00",
        "operating_reset_at": 1751956621,
        "operating": 0
    }
}
```

## 4. Modify a Pre-installed Stage

We will use the method [crm.status.update](../../../api-reference/crm/status/crm-status-update.md) with the following parameters:

- `id` — the stage identifier from the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method,
- `fields[name]` — the new stage name.

{% list tabs %}

- JS
  
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.status.update',
        params: {
            id: stageId,
            fields: {
                NAME: 'New name'
            }
        }
    });

    if (!result.isSuccess) {
        console.error(result.getErrorMessages().join('; '));
    } else {
        console.log('Stage updated');
    }
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getCRMScope()->status()->update(
        $stageId,
        [
            'NAME' => 'New name',
        ]
    );
    ```

- Python

    ```python
    client.crm.status.update(
        stage_id,
        fields={
            "NAME": "New name",
        },
    ).response
    ```

{% endlist %}

As a result, we will receive `true`, indicating that the stage has been successfully modified. If you receive an `error` in the result, refer to the documentation for the method [crm.status.update](../../../api-reference/crm/status/crm-status-update.md) for possible error descriptions.

```json
{
    "result": true,
    "time": {
        "start": 1751956427.737649,
        "finish": 1751956427.799632,
        "duration": 0.06198310852050781,
        "processing": 0.022645950317382812,
        "date_start": "2025-07-08T09:33:47+03:00",
        "date_finish": "2025-07-08T09:33:47+03:00",
        "operating_reset_at": 1751957027,
        "operating": 0
    }
}
```

## 5. Add a New Stage to the Funnel

We will use the method [crm.status.add](../../../api-reference/crm/status/crm-status-add.md) with the following parameters in `fields`:

- `ENTITY_ID` — the identifier of the CRM directory. For smart process stages, the identifier has the format `DYNAMIC_{entityTypeId}_STAGE_{categoryId}`:
	- `{entityTypeId}` — the numeric identifier of the smart process type from the method [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md),
	- `{categoryId}` — the identifier of the funnel from the method [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md).
- `STATUS_ID` — the identifier of the stage. For smart process stages, this field must have the prefix `DT{entityTypeId}_{categoryId}`:
	- `{entityTypeId}` — the numeric identifier of the smart process type from the method [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md),
	- `{categoryId}` — the identifier of the funnel from the method [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md).
- `NAME` — the name of the stage,
- `SORT` — the sorting order of the stage. Sorting affects the order of the stage display in the kanban. The sorting of "In Progress" stages should be the lowest, "Failure" stages the highest. The "Success" stage should have an intermediate sorting value between the sorting values of "In Progress" and "Failure" stages.
- `SEMANTICS` — a parameter indicating the stage's group affiliation. We will specify `F` to create a new stage in the "Failure" group.

{% list tabs %}

- JS
  
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.status.add',
        params: {
            fields: {
                ENTITY_ID: entityId,
                STATUS_ID: `DT${entityTypeId}_${categoryId}:MY_STAGE`,
                NAME: 'My stage',
                SORT: 60,
                SEMANTICS: "F",
            }
        }
    });

    if (!result.isSuccess) {
        console.error(result.getErrorMessages().join('; '));
    } else {
        console.log('New stage ID:', result.getData().result);
    }
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getCRMScope()->status()->add(
        [
            'ENTITY_ID' => $entityId,
            'STATUS_ID' => 'DT' . $entityTypeId . '_' . $categoryId . ':MY_STAGE',
            'NAME' => 'My stage',
            'SORT' => 60,
            'SEMANTICS' => 'F',
        ]
    );
    $newStageId = $result->getId();
    ```

- Python

    ```python
    new_stage_id = client.crm.status.add(
        fields={
            "ENTITY_ID": entity_id,
            "STATUS_ID": f"DT{entity_type_id}_{category_id}:MY_STAGE",
            "NAME": "My stage",
            "SORT": 60,
            "SEMANTICS": "F",
        }
    ).response.result
    ```

{% endlist %}

As a result, we will obtain the ID of the created stage.

```json
{
    "result": 747,
    "time": {
        "start": 1751957029.04664,
        "finish": 1751957029.107654,
        "duration": 0.06101417541503906,
        "processing": 0.02231001853942871,
        "date_start": "2025-07-08T09:43:49+03:00",
        "date_finish": "2025-07-08T09:43:49+03:00",
        "operating_reset_at": 1751957629,
        "operating": 0
    }
}
```

## Code Example

In this example, we create a new funnel in the smart process, change the name of the first pre-installed stage, and add another stage to the "Failure" group. Finally, we call the method [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) again and display a table with the groups of stages.

{% list tabs %}

- JS
  
   ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    async function call(method, params) {
        const result = await $b24.actions.v2.call.make({ method, params });
        if (!result.isSuccess) {
            throw new Error(result.getErrorMessages().join('; '));
        }
        return result.getData().result;
    }

    try {
        // 1. Get entityTypeId by smart process name
        const types = (await call('crm.type.list', {
            filter: { title: 'Equipment procurement' }
        })).types;
        const entityTypeId = types[0].entityTypeId;

        // 2. Create a new funnel
        const category = (await call('crm.category.add', {
            entityTypeId: entityTypeId,
            fields: {
                name: 'New funnel',
                sort: 100
            }
        })).category;
        const categoryId = category.id;
        const entityId = `DYNAMIC_${entityTypeId}_STAGE_${categoryId}`;

        // 3. Get the list of stages
        let stages = await call('crm.status.list', {
            filter: { ENTITY_ID: entityId }
        });

        // 4. Change the first stage
        const firstStageId = stages[0].ID;
        await call('crm.status.update', {
            id: firstStageId,
            fields: { NAME: 'First stage' }
        });

        // 5. Add a new stage
        await call('crm.status.add', {
            fields: {
                ENTITY_ID: entityId,
                STATUS_ID: `DT${entityTypeId}_${categoryId}:MY_STAGE`,
                NAME: 'My stage',
                SORT: 60,
                SEMANTICS: "F"
            }
        });

        // 6. Get and display the final stages table
        stages = await call('crm.status.list', {
            filter: { ENTITY_ID: entityId }
        });
        printStagesTable(stages);
    } catch (error) {
        console.error(error.message);
    }

    function printStagesTable(stages) {
        const columns = {
            'In progress': [],
            'Success': [],
            'Failure': []
        };

        stages.forEach(stage => {
            const semantics = (stage.EXTRA && stage.EXTRA.SEMANTICS) || stage.SEMANTICS;
            if (semantics === 'S') {
                columns['Success'].push(stage.NAME);
            } else if (semantics === 'F') {
                columns['Failure'].push(stage.NAME);
            } else {
                columns['In progress'].push(stage.NAME);
            }
        });

        // Determine the maximum number of rows
        const maxRows = Math.max(
            columns['In progress'].length,
            columns['Success'].length,
            columns['Failure'].length
        );

        // Create an array of objects for console.table
        const tableData = [];

        for (let i = 0; i < maxRows; i++) {
            tableData.push({
                'In progress': columns['In progress'][i] || '',
                'Success': columns['Success'][i] || '',
                'Failure': columns['Failure'][i] || ''
            });
        }

        console.table(tableData);
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

    $crm = $serviceBuilder->getCRMScope();

    try {
        // 1. Get entityTypeId by smart process name
        $types = $crm->type()
            ->list([], ['title' => 'Equipment procurement'])
            ->getTypes();
        $entityTypeId = $types[0]->entityTypeId;

        // 2. Create a new funnel
        $result = $serviceBuilder->core->call(
            'crm.category.add',
            [
                'entityTypeId' => $entityTypeId,
                'fields' => [
                    'name' => 'New funnel',
                    'sort' => 100
                ]
            ]
        );
        $categoryId = $result->getResponseData()->getResult()['category']['id'];
        $entityId = 'DYNAMIC_' . $entityTypeId . '_STAGE_' . $categoryId;

        // 3. Get the list of stages
        $stages = $crm->status()
            ->list([], ['ENTITY_ID' => $entityId])
            ->getStatuses();

        // 4. Change the first stage
        if (!empty($stages)) {
            $firstStageId = $stages[0]->ID;
            $crm->status()->update($firstStageId, ['NAME' => 'First stage']);
        }

        // 5. Add a new stage
        $crm->status()->add([
            'ENTITY_ID' => $entityId,
            'STATUS_ID' => 'DT' . $entityTypeId . '_' . $categoryId . ':MY_STAGE',
            'NAME' => 'My stage',
            'SORT' => 60,
            'SEMANTICS' => 'F',
        ]);

        // 6. Get the final stages table
        $stages = $crm->status()
            ->list([], ['ENTITY_ID' => $entityId])
            ->getStatuses();
    } catch (\Throwable $e) {
        echo 'Error: ' . $e->getMessage();
        exit;
    }

    // Form the stages table
    $columns = [
        'In progress' => [],
        'Success' => [],
        'Failure' => []
    ];

    foreach ($stages as $stage) {
        $semantics = $stage->EXTRA['SEMANTICS'] ?? $stage->SEMANTICS;
        if ($semantics === 'S') {
            $columns['Success'][] = $stage->NAME;
        } elseif ($semantics === 'F') {
            $columns['Failure'][] = $stage->NAME;
        } else {
            $columns['In progress'][] = $stage->NAME;
        }
    }

    // Determine the maximum number of rows
    $maxRows = max(
        count($columns['In progress']),
        count($columns['Success']),
        count($columns['Failure'])
    );

    // Create an array of objects for output
    $tableData = [];

    for ($i = 0; $i < $maxRows; $i++) {
        $tableData[] = [
            'In progress' => $columns['In progress'][$i] ?? '',
            'Success' => $columns['Success'][$i] ?? '',
            'Failure' => $columns['Failure'][$i] ?? ''
        ];
    }

    // Display the table 
    echo "Stages table:\n";
    foreach ($tableData as $row) {
        echo "In progress: " . $row['In progress'] . " | Success: " . $row['Success'] . " | Failure: " . $row['Failure'] . "\n";
    }
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    def print_stages_table(stages):
        columns = {
            "In progress": [],
            "Success": [],
            "Failure": [],
        }

        for stage in stages:
            semantics = (stage.get("EXTRA") or {}).get("SEMANTICS") or stage.get("SEMANTICS")
            if semantics == "S":
                columns["Success"].append(stage["NAME"])
            elif semantics == "F":
                columns["Failure"].append(stage["NAME"])
            else:
                columns["In progress"].append(stage["NAME"])

        max_rows = max(
            len(columns["In progress"]),
            len(columns["Success"]),
            len(columns["Failure"]),
        )

        table_data = []
        for index in range(max_rows):
            table_data.append(
                {
                    "In progress": columns["In progress"][index] if index < len(columns["In progress"]) else "",
                    "Success": columns["Success"][index] if index < len(columns["Success"]) else "",
                    "Failure": columns["Failure"][index] if index < len(columns["Failure"]) else "",
                }
            )

        for row in table_data:
            print(
                "In progress: "
                + row["In progress"]
                + " | Success: "
                + row["Success"]
                + " | Failure: "
                + row["Failure"]
            )

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    try:
        entity_type_id = int(
            client.crm.type.list(
                filter={"title": "Equipment procurement"},
            ).response.result["types"][0]["entityTypeId"]
        )

        category_id = int(
            client.crm.category.add(
                entity_type_id=entity_type_id,
                fields={
                    "name": "New funnel",
                    "sort": 100,
                },
            ).response.result["category"]["id"]
        )
        entity_id = f"DYNAMIC_{entity_type_id}_STAGE_{category_id}"

        stages = client.crm.status.list(
            filter={"ENTITY_ID": entity_id},
        ).response.result

        if stages:
            first_stage_id = int(stages[0]["ID"])
            client.crm.status.update(
                first_stage_id,
                fields={
                    "NAME": "First stage",
                },
            ).response

        client.crm.status.add(
            fields={
                "ENTITY_ID": entity_id,
                "STATUS_ID": f"DT{entity_type_id}_{category_id}:MY_STAGE",
                "NAME": "My stage",
                "SORT": 60,
                "SEMANTICS": "F",
            }
        ).response

        stages = client.crm.status.list(
            filter={"ENTITY_ID": entity_id},
        ).response.result
    except BitrixAPIError as error:
        print(f"Error: {error}")
    else:
        print("Stages table:")
        print_stages_table(stages)
    ```

{% endlist %}
