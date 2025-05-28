# How to Filter Items by Stage Name

> Scope: [`crm, user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: a user with read access to CRM entities

The stage name is not stored in the "Stage" field of the CRM entity. The "Stage" field contains an identifier. You can match the name and identifier of the stage using methods for working with [dictionaries](../../../api-reference/crm/status/index.md) — system fields of the "list" type. To search for items by stage name, we will sequentially execute three methods:

1. [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — retrieve the funnel identifier
2. [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — retrieve the stage identifier in the funnel
3. [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — retrieve the list of items at the stage

## 1. Retrieve the Funnel Identifier

We will use the method [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) with the parameters:
- `entityTypeId` — specify `2` for deals. This is the identifier of the [object type](../../../api-reference/crm/data-types.md#object_type). To find out the identifier of the SPA, execute the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) without parameters.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript  
    BX24.callMethod(
        "crm.category.list",
        {
            entityTypeId: 2,
        },
    );
    ```
- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.category.list',
        [
            'entityTypeId' => 2
        ]
    );
    ```

{% endlist %}

As a result, we obtained the deal funnels. We will identify the required funnel by its name in the `name` field. The funnel identifier will be taken from the `id` field.

```json
{
    "result": {
        "categories": [
            {
                "id": 9,
                "name": "Funnel with Original Name",
                "sort": 200,
                "entityTypeId": 2,
                "isDefault": "N",
                "originId": "",
                "originatorId": ""
            },
            {
                "id": 10,
                "name": "Lead Route",
                "sort": 200,
                "entityTypeId": 2,
                "isDefault": "N",
                "originId": "",
                "originatorId": ""
            },
            {
                "id": 11,
                "name": "Path to Success",
                "sort": 200,
                "entityTypeId": 2,
                "isDefault": "N",
                "originId": "",
                "originatorId": ""
            },
            {
                "id": 0,
                "name": "General",
                "sort": 300,
                "entityTypeId": 2,
                "isDefault": "Y"
            }
        ]
    },
    "total": 4,
}
```

## 2. Retrieve the Stage Identifier

We will use the method [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) with the filter:

- `ENTITY_ID` — specify `DEAL_STAGE_10`, where `10` is the funnel identifier obtained in step 1. 
To obtain the stages of the SPA, use a formula like `DYNAMIC_185_STAGE_11`, where `185` is the `ID` of the SPA, and `11` is the `ID` of the funnel.
If the `ID` of the funnel is `0`, make the request for stages without adding `_ID`.

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        "crm.status.list",
        {
            filter: { "ENTITY_ID": "DEAL_STAGE_10"}
        },
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.status.list',
        [
            'filter' => [
                'ENTITY_ID' => 'DEAL_STAGE_10'
            ]
        ]
    );
    ```

{% endlist %}

As a result, we obtained a list of stages. We will identify the required stage by its name in the `NAME` field. The stage identifier will be taken from the `STATUS_ID` field.

```json
{
    "result": [
        {
            "ID": "331",
            "ENTITY_ID": "DEAL_STAGE_10",
            "STATUS_ID": "C10:NEW",
            "NAME": "New",
            "NAME_INIT": "New",
            "SORT": "10",
            "SYSTEM": "Y",
            "CATEGORY_ID": "5",
            "COLOR": "#39A8EF",
            "SEMANTICS": null,
            "EXTRA": {
                "SEMANTICS": "process",
                "COLOR": "#39A8EF"
            }
        },
        {
            "ID": "333",
            "ENTITY_ID": "DEAL_STAGE_10",
            "STATUS_ID": "C10:PREPARATION",
            "NAME": "Document Preparation",
            "NAME_INIT": "",
            "SORT": "20",
            "SYSTEM": "N",
            "CATEGORY_ID": "5",
            "COLOR": "#2FC6F6",
            "SEMANTICS": null,
            "EXTRA": {
                "SEMANTICS": "process",
                "COLOR": "#2FC6F6"
            }
        },
        {
            "ID": "335",
            "ENTITY_ID": "DEAL_STAGE_10",
            "STATUS_ID": "C10:PREPAYMENT_INVOICE",
            "NAME": "Agreement",
            "NAME_INIT": "",
            "SORT": "30",
            "SYSTEM": "N",
            "CATEGORY_ID": "5",
            "COLOR": "#55d0e0",
            "SEMANTICS": null,
            "EXTRA": {
                "SEMANTICS": "process",
                "COLOR": "#55d0e0"
            }
        },
        {
            "ID": "337",
            "ENTITY_ID": "DEAL_STAGE_10",
            "STATUS_ID": "C10:EXECUTING",
            "NAME": "In Progress",
            "NAME_INIT": "",
            "SORT": "40",
            "SYSTEM": "N",
            "CATEGORY_ID": "5",
            "COLOR": "#47E4C2",
            "SEMANTICS": null,
            "EXTRA": {
                "SEMANTICS": "process",
                "COLOR": "#47E4C2"
            }
        },
        {
            "ID": "339",
            "ENTITY_ID": "DEAL_STAGE_10",
            "STATUS_ID": "C10:FINAL_INVOICE",
            "NAME": "Final Invoice",
            "NAME_INIT": "",
            "SORT": "50",
            "SYSTEM": "N",
            "CATEGORY_ID": "5",
            "COLOR": "#FFA900",
            "SEMANTICS": null,
            "EXTRA": {
                "SEMANTICS": "process",
                "COLOR": "#FFA900"
            }
        },
        {
            "ID": "341",
            "ENTITY_ID": "DEAL_STAGE_10",
            "STATUS_ID": "C10:WON",
            "NAME": "Deal Won",
            "NAME_INIT": "Deal Won",
            "SORT": "60",
            "SYSTEM": "Y",
            "CATEGORY_ID": "5",
            "COLOR": "#7BD500",
            "SEMANTICS": "S",
            "EXTRA": {
                "SEMANTICS": "success",
                "COLOR": "#7BD500"
            }
        },
        {
            "ID": "343",
            "ENTITY_ID": "DEAL_STAGE_10",
            "STATUS_ID": "C10:LOSE",
            "NAME": "Deal Lost",
            "NAME_INIT": "Deal Lost",
            "SORT": "70",
            "SYSTEM": "Y",
            "CATEGORY_ID": "5",
            "COLOR": "#FF5752",
            "SEMANTICS": "F",
            "EXTRA": {
                "SEMANTICS": "failure",
                "COLOR": "#FF5752"
            }
        },
        {
            "ID": "345",
            "ENTITY_ID": "DEAL_STAGE_10",
            "STATUS_ID": "C10:APOLOGY",
            "NAME": "Analysis of Failure Reason",
            "NAME_INIT": "",
            "SORT": "80",
            "SYSTEM": "N",
            "CATEGORY_ID": "5",
            "COLOR": "#FF5752",
            "SEMANTICS": "F",
            "EXTRA": {
                "SEMANTICS": "apology",
                "COLOR": "#FF5752"
            }
        }
    ],
    "total": 8,
}
```

## 3. Retrieve the List of Items at the Stage

We will use the method [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) with the parameters:
- `entityTypeId` — specify `2` for deals. This is the identifier of the [object type](../../../api-reference/crm/data-types.md#object_type). To find out the identifier of the SPA, execute the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) without parameters.
- `filter[stageId]` — specify `C10:PREPAYMENT_INVOICE`. This is the stage identifier obtained in step 2.
- `select[]` — specify the fields of the items that we want to retrieve. Without the `select` parameter, all fields, including custom ones, will be returned.

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'crm.item.list',
        {
            entityTypeId: 2,
            select: [
                "id", 
                "title",
                "assignedById", 
                "opportunity", 
            ],
            filter: {
                "stageId": ["C10:PREPAYMENT_INVOICE"],
            },
        },
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.list',
        [
            'entityTypeId' => 2,
            'select' => [
                "id",
                "title",
                "assignedById",
                "opportunity",
            ],
            'filter' => [
                "stageId" => ["C10:PREPAYMENT_INVOICE"],
            ],
        ]
    );
    ```

{% endlist %}

As a result, we obtained a list of items at the requested stage.

```json
{
    "result": {
        "items": [
            {
                "id": 5111,
                "assignedById": 1,
                "title": "Purchase of Stoves",
                "opportunity": 500
            },
            {
                "id": 5199,
                "assignedById": 29,
                "title": "Purchase of Heaters",
                "opportunity": 250
            },
            {
                "id": 5257,
                "assignedById": 29,
                "title": "Purchase of Bread Makers",
                "opportunity": 200
            },
            {
                "id": 5273,
                "assignedById": 29,
                "title": "Purchase of Cars",
                "opportunity": 0
            },
            {
                "id": 5317,
                "assignedById": 29,
                "title": "Purchase of Blenders",
                "opportunity": 100
            }
        ]
    },
    "total": 5,
}
```

## Retrieve the Responsible Person's Data

In the obtained result, the `ID` of the employee responsible for the item is specified. To display the first and last name of the employee, we will use the method [user.get](../../../api-reference/user/user-get.md) with the filter:

- `ID` — specify the value from the `assignedById` parameter obtained in step 3.

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        "user.get",
        {
            "ID": 29
        },
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.get',
        [
            'ID' => 29
        ]
    );
    ```

{% endlist %}

As a result, we will receive data about the employee, including the fields `NAME` and `LAST_NAME`.

```json
{
    "result": [
        {
            "ID": "29",
            "ACTIVE": true,
            "NAME": "Vadim",
            "LAST_NAME": "Valeev",
            "SECOND_NAME": "",
            "EMAIL": "v.r.valeev@bitrix.com",
            "LAST_LOGIN": "2025-05-15T13:06:54+00:00",
            "DATE_REGISTER": "2024-07-15T00:00:00+00:00",
            "TIME_ZONE": "",
            "IS_ONLINE": "Y",
            "TIME_ZONE_OFFSET": "7200",
            "TIMESTAMP_X": {
            },
            "LAST_ACTIVITY_DATE": {
            },
            "PERSONAL_GENDER": "",
            "PERSONAL_WWW": "",
            "PERSONAL_BIRTHDAY": "2000-07-14T00:00:00+00:00",
            "PERSONAL_MOBILE": "",
            "PERSONAL_CITY": "",
            "WORK_PHONE": "",
            "WORK_POSITION": "",
            "UF_EMPLOYMENT_DATE": "",
            "UF_DEPARTMENT": [1],
            "USER_TYPE": "employee"
        },
    ],
}
```

## Code Example

{% list tabs %}

- JS
  
    ```javascript
    // Step 1: Request the funnel name from the user
    let funnelName = prompt("Enter the name of the deal funnel:");

    // Step 2: Retrieve the list of funnels
    BX24.callMethod(
        "crm.category.list",
        {
            entityTypeId: 2,
        },
        function (result) {
            if (result.error()) {
                console.error(result.error().ex);
                return;
            }

            let categories = result.data().categories;
            let selectedFunnel = categories.find(cat => cat.name === funnelName);

            if (!selectedFunnel) {
                alert("Funnel not found.");
                return;
            }

            let funnelId = selectedFunnel.id;

            // Step 3: Request the stage name from the user
            let stageName = prompt("Enter the stage name:");

            // Step 4: Retrieve the list of stages for the selected funnel
            let entityID = funnelId === 0 ? "DEAL_STAGE" : `DEAL_STAGE_${funnelId}`;

            BX24.callMethod(
                "crm.status.list",
                {
                    filter: { "ENTITY_ID": entityID }
                },
                function (result) {
                    if (result.error()) {
                        console.error(result.error().ex);
                        return;
                    }

                    let stages = result.data();
                    let selectedStage = stages.find(stage => stage.NAME === stageName);

                    if (!selectedStage) {
                        alert("Stage not found.");
                        return;
                    }

                    let stageId = selectedStage.STATUS_ID;

                    // Step 5: Retrieve the list of deals at the selected stage
                    BX24.callMethod(
                        "crm.item.list",
                        {
                            entityTypeId: 2,
                            select: ["id", "title", "assignedById", "opportunity"],
                            filter: {
                                "stageId": stageId,
                            },
                        },
                        function (result) {
                            if (result.error()) {
                                console.error(result.error().ex);
                                return;
                            }

                            let deals = result.data().items;
                            let uniqueResponsibleIds = [...new Set(deals.map(deal => deal.assignedById))];

                            let userMap = {};

                            // Step 6: Retrieve user information
                            uniqueResponsibleIds.forEach(userId => {
                                BX24.callMethod(
                                    "user.get",
                                    {
                                        "ID": userId
                                    },
                                    function (userResult) {
                                        if (userResult.error()) {
                                            console.error(userResult.error().ex);
                                            return;
                                        }

                                        let user = userResult.data()[0];
                                        userMap[userId] = {
                                            name: user.NAME,
                                            lastName: user.LAST_NAME
                                        };
                                    }
                                );
                            });

                            // Step 7: Output results to the console in a text table format
                            setTimeout(() => {
                                let table = [];

                                // Header
                                table.push([
                                    "Deal ID",
                                    "Title",
                                    "Responsible Name",
                                    "Responsible Last Name",
                                    "Expected Income"
                                ]);

                                // Data rows
                                deals.forEach(deal => {
                                    let responsible = userMap[deal.assignedById] || { name: "Unknown", lastName: "Unknown" };
                                    table.push([
                                        deal.id,
                                        deal.title,
                                        responsible.name,
                                        responsible.lastName,
                                        deal.opportunity || 0
                                    ]);
                                });

                                // Output the table to the console
                                console.table(table);
                            }, 1000); // Delay for all requests to complete
                        }
                    );
                }
            );
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    // Step 1: Request the funnel name from the user
    $funnelName = readline("Enter the name of the deal funnel: ");

    // Step 2: Retrieve the list of funnels
    $result = CRest::call(
        'crm.category.list',
        [
            'entityTypeId' => 2
        ]
    );

    if (!empty($result['error'])) {
        echo "Error: " . $result['error_description'] . "\n";
        exit;
    }

    $categories = $result['result']['categories'];
    $selectedFunnel = null;

    foreach ($categories as $category) {
        if ($category['NAME'] === $funnelName) {
            $selectedFunnel = $category;
            break;
        }
    }

    if (!$selectedFunnel) {
        echo "Funnel not found.\n";
        exit;
    }

    $funnelId = $selectedFunnel['ID'];

    // Step 3: Request the stage name from the user
    $stageName = readline("Enter the stage name: ");

    // Step 4: Retrieve the list of stages for the selected funnel
    $entityID = $funnelId === 0 ? "DEAL_STAGE" : "DEAL_STAGE_{$funnelId}";

    $result = CRest::call(
        'crm.status.list',
        [
            'filter' => [
                'ENTITY_ID' => $entityID
            ]
        ]
    );

    if (!empty($result['error'])) {
        echo "Error: " . $result['error_description'] . "\n";
        exit;
    }

    $stages = $result['result'];
    $selectedStage = null;

    foreach ($stages as $stage) {
        if ($stage['NAME'] === $stageName) {
            $selectedStage = $stage;
            break;
        }
    }

    if (!$selectedStage) {
        echo "Stage not found.\n";
        exit;
    }

    $stageId = $selectedStage['STATUS_ID'];

    // Step 5: Retrieve the list of deals at the selected stage
    $result = CRest::call(
        'crm.item.list',
        [
            'entityTypeId' => 2,
            'select' => [
                "id",
                "title",
                "assignedById",
                "opportunity"
            ],
            'filter' => [
                "stageId" => $stageId
            ]
        ]
    );

    if (!empty($result['error'])) {
        echo "Error: " . $result['error_description'] . "\n";
        exit;
    }

    $deals = $result['result']['items'];
    $uniqueResponsibleIds = array_unique(array_column($deals, 'assignedById'));

    $userMap = [];

    // Step 6: Retrieve user information
    foreach ($uniqueResponsibleIds as $userId) {
        $result = CRest::call(
            'user.get',
            [
                'ID' => $userId
            ]
        );

        if (!empty($result['error'])) {
            echo "Error: " . $result['error_description'] . "\n";
            continue;
        }

        $user = $result['result'][0];
        $userMap[$userId] = [
            'name' => $user['NAME'],
            'lastName' => $user['LAST_NAME']
        ];
    }

    // Step 7: Output results in a text table format
    $table = [];

    // Header
    $table[] = [
        "Deal ID",
        "Title",
        "Responsible Name",
        "Responsible Last Name",
        "Expected Income"
    ];

    // Data rows
    foreach ($deals as $deal) {
        $responsible = $userMap[$deal['assignedById']] ?? ['name' => 'Unknown', 'lastName' => 'Unknown'];
        $table[] = [
            $deal['id'],
            $deal['title'],
            $responsible['name'],
            $responsible['lastName'],
            $deal['opportunity'] ?? 0
        ];
    }

    // Output the table
    foreach ($table as $row) {
        echo implode("\t", $row) . "\n";
    }
    ```

{% endlist %}