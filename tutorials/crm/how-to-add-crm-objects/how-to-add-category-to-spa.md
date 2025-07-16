# How to Create a New Sales Funnel with Stages in a Smart Process

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute methods: users with administrative access to the CRM section

Sales funnels allow you to divide work in CRM into different stages. For example, a sale can consist of three funnels: sales, delivery, and post-sale service. Access permissions and the view of the CRM entity card can be configured for each funnel.

To create a new funnel in a smart process, we will sequentially execute the following methods:

1. [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) — retrieve the numeric identifier of the smart process type.
2. [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) — create a new funnel.
3. [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — retrieve the pre-installed stages of the new funnel.
4. [crm.status.update](../../../api-reference/crm/status/crm-status-update.md) — modify the pre-installed stage.
5. [crm.status.add](../../../api-reference/crm/status/crm-status-add.md) — add a new stage.

## 1. Retrieve the Smart Process Identifier

Use the method [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) with a filter by the smart process `title`.

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.type.list',
        {
            filter: { title: 'Equipment Purchase' }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
                return;
            }
            var types = result.data().types;
            if (types.length > 0) {
                var entityTypeId = types[0].entityTypeId;
                console.log('entityTypeId:', entityTypeId);
            }
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');
    $result = CRest::call(
        'crm.type.list',
        [ 'filter' => [ 'title' => 'Equipment Purchase' ] ]
    );
    $entityTypeId = $result['result']['types'][0]['entityTypeId'];
    ```

{% endlist %}

As a result, we will obtain and save the `entityTypeId` of the required smart process.

```json
{
    "result": {
        "types": [
            {
                "id": 7,
                "title": "Equipment Purchase",
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

Use the method [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) with the following parameters:

- `entityTypeId` — the numeric identifier of the type from the method [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md),
- `fields[name]` — the name of the funnel,
- `fields[sort]` — the sorting of the funnel. Sorting affects the position of the funnel in the list.

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.category.add',
        {
            entityTypeId: entityTypeId, 
            fields: {
                name: 'New Funnel',
                sort: 100
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
                return;
            }
            var categoryId = result.data().category.id;
            console.log('categoryId:', categoryId);
        }
    );
    ```

- PHP
  
    ```php
    $result = CRest::call(
        'crm.category.add',
        [
            'entityTypeId' => $entityTypeId,
            'fields' => [
                'name' => 'New Funnel',
                'sort' => 100,
            ]
        ]
    );
    $categoryId = $result['result']['category']['id'];
    ```

{% endlist %}

As a result, we will obtain and save the `id` of the created funnel.

```json
{
    "result": {
        "category": {
            "id": 39,
            "name": "New Funnel",
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

Use the method [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) with the filter:

- `ENTITY_ID` — the identifier of the CRM directory. For smart process stages, the identifier has the format `DYNAMIC_{entityTypeId}_STAGE_{categoryId}`:
	- `{entityTypeId}` — the numeric identifier of the smart process type from the method [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md),
	- `{categoryId}` — the identifier of the funnel from the method [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md).

{% list tabs %}

- JS
  
    ```javascript
    var entityId = `DYNAMIC_${entityTypeId}_STAGE_${categoryId}`;
    BX24.callMethod(
        'crm.status.list',
        {
            filter: { ENTITY_ID: entityId }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
                return;
            }
            var stages = result.data();
            console.log('Stages:', stages);
        }
    );
    ```

- PHP
  
    ```php
    $entityId = "DYNAMIC_{$entityTypeId}_STAGE_{$categoryId}";
    $result = CRest::call(
        'crm.status.list',
        [ 'filter' => [ 'ENTITY_ID' => $entityId ] ]
    );
    $stages = $result['result'];
    ```

{% endlist %}

As a result, we will obtain the pre-installed stages of the funnel. By default, each new funnel has five stages:

- three stages "In Progress" `SEMANTICS: ""`,
- one stage "Success" `SEMANTICS: "S"`,
- one stage "Failure" `SEMANTIC": "F"`.

Each funnel must have at least one stage from each group. There can only be one successful stage in the funnel.

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

## 4. Modify the Pre-installed Stage

Use the method [crm.status.update](../../../api-reference/crm/status/crm-status-update.md) with the parameters:

- `id` — the identifier of the stage from the method [crm.status.list](../../../api-reference/crm/status/crm-status-list.md),
- `fields[name]` — the new name of the stage.

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.status.update',
        {
            id: stageId, 
            fields: {
                NAME: 'New Name'
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
                return;
            }
            console.log('Stage updated');
        }
    );
    ```

- PHP
  
    ```php
    $result = CRest::call(
        'crm.status.update',
        [
            'id' => $stageId,
            'fields' => [
                'NAME' => 'New Name',
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive `true`, indicating that the stage has been successfully modified. If you receive an `error` as a result, review the possible error descriptions in the documentation for the method [crm.status.update](../../../api-reference/crm/status/crm-status-update.md).

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

Use the method [crm.status.add](../../../api-reference/crm/status/crm-status-add.md) with the `fields` parameters:

- `ENTITY_ID` — the identifier of the CRM directory. For smart process stages, the identifier has the format `DYNAMIC_{entityTypeId}_STAGE_{categoryId}`:
	- `{entityTypeId}` — the numeric identifier of the smart process type from the method [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md),
	- `{categoryId}` — the identifier of the funnel from the method [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md).
- `STATUS_ID` — the identifier of the stage. For smart process stages, the field must have the prefix `DT{entityTypeId}_{categoryId}`:
	- `{entityTypeId}` — the numeric identifier of the smart process type from the method [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md),
	- `{categoryId}` — the identifier of the funnel from the method [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md).
- `NAME` — the name of the stage,
- `SORT` — the sorting of the stage. Sorting affects the display order of the stage in the kanban. The sorting of "In Progress" stages should be the lowest, "Failure" stages the highest. The "Success" stage should have an intermediate sorting value between the sorting values of "In Progress" and "Failure" stages.
- `SEMANTICS` — the parameter indicating the stage's group membership. We will specify `F` to create a new stage in the "Failure" group.

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.status.add',
        {
            fields: {
                ENTITY_ID: entityId,
                STATUS_ID: `DT${entityTypeId}_${categoryId}:MY_STAGE`,
                NAME: 'My Stage',
                SORT: 60,
                SEMANTICS: "F",
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
                return;
            }
            console.log('New stage ID:', result.data());
        }
    );
    ```

- PHP
  
    ```php
    $result = CRest::call(
        'crm.status.add',
        [
            'fields' => [
                'ENTITY_ID' => $entityId,
                'STATUS_ID' => 'DT' . $entityTypeId . '_' . $categoryId . ':MY_STAGE',
                'NAME' => 'My Stage',
                'SORT' => 60,
                'SEMANTICS' => 'F',
            ]
        ]
    );
    $newStageId = $result['result'];
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

In this example, we create a new funnel in a smart process, change the name of the first pre-installed stage, and add another stage to the "Failure" group. Finally, we call the method [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) again and display a table with the groups of stages.

{% list tabs %}

- JS
  
   ```javascript
    // 1. Retrieve entityTypeId by the smart process title
    BX24.callMethod('crm.type.list', {
        filter: {
            title: 'Equipment Purchase'
        }
    }, function(result) {
        if (result.error()) return;
        var entityTypeId = result.data().types[0].entityTypeId;

        // 2. Create a new funnel
        BX24.callMethod('crm.category.add', {
            entityTypeId: entityTypeId,
            fields: {
                name: 'New Funnel',
                sort: 100
            }
        }, function(result) {
            if (result.error()) return;
            var categoryId = result.data().category.id;
            var entityId = `DYNAMIC_${entityTypeId}_STAGE_${categoryId}`;

            // 3. Retrieve the list of stages
            BX24.callMethod('crm.status.list', {
                filter: {
                    ENTITY_ID: entityId
                }
            }, function(result) {
                if (result.error()) return;
                var stages = result.data();

                // 4. Modify the first stage
                var firstStageId = stages[0].ID;
                BX24.callMethod('crm.status.update', {
                    id: firstStageId,
                    fields: {
                        NAME: 'First Stage'
                    }
                }, function() {
                    // 5. Add a new stage
                    BX24.callMethod('crm.status.add', {
                        fields: {
                            ENTITY_ID: entityId,
                            STATUS_ID: `DT${entityTypeId}_${categoryId}:MY_STAGE`,
                            NAME: 'My Stage',
                            SORT: 60,
                            SEMANTICS: "F"
                        }
                    }, function() {
                        // 6. Retrieve and display the final table of stages
                        BX24.callMethod('crm.status.list', {
                            filter: {
                                ENTITY_ID: entityId
                            }
                        }, function(result) {
                            if (result.error()) return;
                            printStagesTable(result.data());
                        });
                    });
                });
            });
        });
    });

    function printStagesTable(stages) {
        const columns = {
            'In Progress': [],
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
                columns['In Progress'].push(stage.NAME);
            }
        });

        // Determine the maximum number of rows
        const maxRows = Math.max(
            columns['In Progress'].length,
            columns['Success'].length,
            columns['Failure'].length
        );

        // Create an array of objects for console.table
        const tableData = [];

        for (let i = 0; i < maxRows; i++) {
            tableData.push({
                'In Progress': columns['In Progress'][i] || '',
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
    require_once('crest.php');

    // 1. Retrieve entityTypeId by the smart process title
    $result = CRest::call(
        'crm.type.list',
        [
            'filter' => [
                'title' => 'Equipment Purchase'
            ]
        ]
    );

    if ($result['error']) {
        echo 'Error: ' . $result['error_description'];
        exit;
    }

    $entityTypeId = $result['result']['types'][0]['entityTypeId'];

    // 2. Create a new funnel
    $result = CRest::call(
        'crm.category.add',
        [
            'entityTypeId' => $entityTypeId,
            'fields' => [
                'name' => 'New Funnel',
                'sort' => 100
            ]
        ]
    );

    if ($result['error']) {
        echo 'Error: ' . $result['error_description'];
        exit;
    }

    $categoryId = $result['result']['category']['id'];
    $entityId = 'DYNAMIC_' . $entityTypeId . '_STAGE_' . $categoryId;

    // 3. Retrieve the list of stages
    $result = CRest::call(
        'crm.status.list',
        [
            'filter' => [
                'ENTITY_ID' => $entityId
            ]
        ]
    );

    if ($result['error']) {
        echo 'Error: ' . $result['error_description'];
        exit;
    }

    $stages = $result['result'];

    // 4. Modify the first stage
    if (!empty($stages)) {
        $firstStageId = $stages[0]['ID'];
        $result = CRest::call(
            'crm.status.update',
            [
                'id' => $firstStageId,
                'fields' => [
                    'NAME' => 'First Stage'
                ]
            ]
        );

        if ($result['error']) {
            echo 'Error: ' . $result['error_description'];
            exit;
        }
    }

    // 5. Add a new stage
    $result = CRest::call(
        'crm.status.add',
        [
            'fields' => [
                'ENTITY_ID' => $entityId,
                'STATUS_ID' => 'DT' . $entityTypeId . '_' . $categoryId . ':MY_STAGE',
                'NAME' => 'My Stage',
                'SORT' => 60,
                'SEMANTICS' => 'F',
            ]
        ]
    );

    if ($result['error']) {
        echo 'Error: ' . $result['error_description'];
        exit;
    }

    // 6. Retrieve and display the final table of stages
    $result = CRest::call(
        'crm.status.list',
        [
            'filter' => [
                'ENTITY_ID' => $entityId
            ]
        ]
    );

    if ($result['error']) {
        echo 'Error: ' . $result['error_description'];
        exit;
    }

    $stages = $result['result'];

    // Form the stages table
    $columns = [
        'In Progress' => [],
        'Success' => [],
        'Failure' => []
    ];

    foreach ($stages as $stage) {
        $semantics = ($stage['EXTRA'] && $stage['EXTRA']['SEMANTICS']) ? $stage['EXTRA']['SEMANTICS'] : $stage['SEMANTICS'];
        if ($semantics === 'S') {
            $columns['Success'][] = $stage['NAME'];
        } elseif ($semantics === 'F') {
            $columns['Failure'][] = $stage['NAME'];
        } else {
            $columns['In Progress'][] = $stage['NAME'];
        }
    }

    // Determine the maximum number of rows
    $maxRows = max(
        count($columns['In Progress']),
        count($columns['Success']),
        count($columns['Failure'])
    );

    // Create an array of objects for output
    $tableData = [];

    for ($i = 0; $i < $maxRows; $i++) {
        $tableData[] = [
            'In Progress' => $columns['In Progress'][$i] ?? '',
            'Success' => $columns['Success'][$i] ?? '',
            'Failure' => $columns['Failure'][$i] ?? ''
        ];
    }

    // Output the table 
    echo "Stages Table:\n";
    foreach ($tableData as $row) {
        echo "In Progress: " . $row['In Progress'] . " | Success: " . $row['Success'] . " | Failure: " . $row['Failure'] . "\n";
    }
    ```

{% endlist %}