# How to Filter Items by Stage Name

> Scope: [`crm, user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: a user with read access to CRM entities

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

The stage name is not stored in the "Stage" field of the CRM object. The "Stage" field contains an identifier. You can correlate the name and identifier of the stage using methods for working with [dictionaries](../../../api-reference/crm/status/index.md) — system fields of the "list" type. To search for items by stage name, we will sequentially execute three methods:

1. [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — retrieve the funnel identifier
2. [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — retrieve the stage identifier in the funnel
3. [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — retrieve the list of items at the stage

## 1. Retrieve the Funnel Identifier

We will use the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with the following parameters:
- `entityTypeId` — set to `2` for deals. This is the identifier for the [object type](../../../api-reference/crm/data-types.md#object_type). To find out the `entityTypeId` of the SPA, execute the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method without parameters.

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript  
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: "crm.category.list",
        params: {
            entityTypeId: 2,
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

    $result = $serviceBuilder->core->call(
        'crm.category.list',
        [
            'entityTypeId' => 2
        ]
    );
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    result = client.crm.category.list(
        entity_type_id=2,
    ).response.result
    ```

{% endlist %}

As a result, we obtained the deal funnels. We will identify the required funnel by its name in the `name` field. The funnel identifier will be taken from the `id` field.

```json
{
    "result": {
        "categories": [
            {
                "id": 9,
                "name": "Funnel with original name",
                "sort": 200,
                "entityTypeId": 2,
                "isDefault": "N",
                "originId": "",
                "originatorId": ""
            },
            {
                "id": 10,
                "name": "Lead route",
                "sort": 200,
                "entityTypeId": 2,
                "isDefault": "N",
                "originId": "",
                "originatorId": ""
            },
            {
                "id": 11,
                "name": "Success path",
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

We will use the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method with the filter:

- `ENTITY_ID` — set to `DEAL_STAGE_10`, where `10` is the funnel identifier obtained in step 1. 
To obtain the stages of the SPA, use a formula of the form `DYNAMIC_185_STAGE_11`, where `185` is the `entityTypeId` of the SPA, and `11` is the funnel `ID`. 
If the funnel `ID` is `0`, make the request for stages without adding `_ID`.

{% list tabs %}

- JS
  
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: "crm.status.list",
        params: {
            filter: { "ENTITY_ID": "DEAL_STAGE_10"}
        }
    });
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getCRMScope()->status()->list(
        [],
        [
            'ENTITY_ID' => 'DEAL_STAGE_10'
        ]
    );
    ```

- Python

    ```python
    result = client.crm.status.list(
        filter={
            "ENTITY_ID": "DEAL_STAGE_10",
        }
    ).response.result
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
            "NAME": "Document preparation",
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
            "NAME": "Approval",
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
            "NAME": "In progress",
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
            "NAME": "Final invoice",
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
            "NAME": "Deal successful",
            "NAME_INIT": "Deal successful",
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
            "NAME": "Deal failed",
            "NAME_INIT": "Deal failed",
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
            "NAME": "Failure reason analysis",
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

Use the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method with the following parameters:
- `entityTypeId` — specify `2` for deals. This is the [object type](../../../api-reference/crm/data-types.md#object_type) identifier. To find the `entityTypeId` of the SPA, call the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method without parameters.
- `filter[stageId]` — specify `C10:PREPAYMENT_INVOICE`. This is the stage identifier obtained in step 2.
- `select[]` — specify the item fields you wish to retrieve. Without the `select` parameter, all fields, including custom fields, will be returned.

{% list tabs %}

- JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.item.list',
        params: {
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
        }
    });
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getCRMScope()->item()->list(
        2,
        [],
        [
            "stageId" => ["C10:PREPAYMENT_INVOICE"],
        ],
        [
            "id",
            "title",
            "assignedById",
            "opportunity",
        ]
    );
    ```

- Python

    ```python
    result = client.crm.item.list(
        entity_type_id=2,
        select=["id", "title", "assignedById", "opportunity"],
        filter={
            "stageId": ["C10:PREPAYMENT_INVOICE"],
        },
    ).response.result
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
                "title": "Purchase of ovens",
                "opportunity": 500
            },
            {
                "id": 5199,
                "assignedById": 29,
                "title": "Purchase of heaters",
                "opportunity": 250
            },
            {
                "id": 5257,
                "assignedById": 29,
                "title": "Purchase of bread makers",
                "opportunity": 200
            },
            {
                "id": 5273,
                "assignedById": 29,
                "title": "Purchase of cars",
                "opportunity": 0
            },
            {
                "id": 5317,
                "assignedById": 29,
                "title": "Purchase of blenders",
                "opportunity": 100
            }
        ]
    },
    "total": 5,
}
```

## Retrieve Responsible Person's Data

In the obtained result, the `ID` of the employee responsible for the item is indicated. To display the first and last name of the employee, we will use the [user.get](../../../api-reference/user/user-get.md) method with the filter:

- `ID` — set to the value from the `assignedById` parameter obtained in step 3.

{% list tabs %}

- JS
  
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: "user.get",
        params: {
            "ID": 29
        }
    });
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getUserScope()->user()->get(
        [],
        ['ID' => 29]
    );
    ```

- Python

    ```python
    result = client.user.get(
        filter={"ID": 29},
    ).response.result
    ```

{% endlist %}

As a result, we will obtain the employee data, including the `NAME` and `LAST_NAME` fields.

```json
    {
        "result": [
            {
                "ID": "29",
                "ACTIVE": true,
                "NAME": "Klaus",
                "LAST_NAME": "Weber",
                "SECOND_NAME": "",
                "EMAIL": "v.r.valeev@bitrix.com",
                "LAST_LOGIN": "2025-05-15T13:06:54+00:00",
                "DATE_REGISTER": "2024-07-15T00:00:00+00:00",
                "TIME_ZONE": "",
                "IS_ONLINE": "Y",
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

    try {
        const rl = createInterface({ input: process.stdin, output: process.stdout });

        // Step 1: Request funnel name from user
        let funnelName = await rl.question("Enter deal funnel name: ");

        // Step 2: Get list of funnels
        let categories = (await call("crm.category.list", { entityTypeId: 2 })).categories;
        let selectedFunnel = categories.find(cat => cat.name === funnelName);

        if (!selectedFunnel) {
            console.log("Funnel not found.");
            rl.close();
        } else {
            let funnelId = selectedFunnel.id;

            // Step 3: Request stage name from user
            let stageName = await rl.question("Enter stage name: ");
            rl.close();

            // Step 4: Get list of stages for the selected funnel
            let entityID = funnelId === 0 ? "DEAL_STAGE" : `DEAL_STAGE_${funnelId}`;

            let stages = await call("crm.status.list", { filter: { "ENTITY_ID": entityID } });
            let selectedStage = stages.find(stage => stage.NAME === stageName);

            if (!selectedStage) {
                console.log("Stage not found.");
            } else {
                let stageId = selectedStage.STATUS_ID;

                // Step 5: Get list of deals at the selected stage
                let deals = (await call("crm.item.list", {
                    entityTypeId: 2,
                    select: ["id", "title", "assignedById", "opportunity"],
                    filter: {
                        "stageId": stageId,
                    },
                })).items;

                let uniqueResponsibleIds = [...new Set(deals.map(deal => deal.assignedById))];

                let userMap = {};

                // Step 6: Get user information
                for (const userId of uniqueResponsibleIds) {
                    let users = await call("user.get", { "ID": userId });
                    let user = users[0];
                    userMap[userId] = {
                        name: user.NAME,
                        lastName: user.LAST_NAME
                    };
                }

                // Step 7: Output results to console as a text table
                let table = [];

                // Header
                table.push([
                    "Deal ID",
                    "Name",
                    "Responsible first name",
                    "Responsible last name",
                    "Expected revenue"
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

                // Output table to console
                console.table(table);
            }
        }
    } catch (error) {
        console.error(error.message);
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

    $crm = $serviceBuilder->getCRMScope();

    // Step 1: Request funnel name from user
    $funnelName = readline("Enter deal funnel name: ");

    // Step 2: Get list of funnels
    $categories = $serviceBuilder->core->call(
        'crm.category.list',
        [
            'entityTypeId' => 2
        ]
    )->getResponseData()->getResult()['categories'];

    $selectedFunnel = null;

    foreach ($categories as $category) {
        if ($category['name'] === $funnelName) {
            $selectedFunnel = $category;
            break;
        }
    }

    if (!$selectedFunnel) {
        echo "Funnel not found.\n";
        exit;
    }

    $funnelId = $selectedFunnel['id'];

    // Step 3: Request stage name from user
    $stageName = readline("Enter stage name: ");

    // Step 4: Get list of stages for the selected funnel
    $entityID = $funnelId === 0 ? "DEAL_STAGE" : "DEAL_STAGE_{$funnelId}";

    $stages = $crm->status()->list(
        [],
        [
            'ENTITY_ID' => $entityID
        ]
    )->getStatuses();

    $selectedStage = null;

    foreach ($stages as $stage) {
        if ($stage->NAME === $stageName) {
            $selectedStage = $stage;
            break;
        }
    }

    if (!$selectedStage) {
        echo "Stage not found.\n";
        exit;
    }

    $stageId = $selectedStage->STATUS_ID;

    // Step 5: Get list of deals at the selected stage
    $deals = $crm->item()->list(
        2,
        [],
        [
            "stageId" => $stageId
        ],
        [
            "id",
            "title",
            "assignedById",
            "opportunity"
        ]
    )->getItems();

    $uniqueResponsibleIds = [];
    foreach ($deals as $deal) {
        $uniqueResponsibleIds[$deal->assignedById] = $deal->assignedById;
    }

    $userMap = [];

    // Step 6: Get user information
    foreach ($uniqueResponsibleIds as $userId) {
        $users = $serviceBuilder->getUserScope()->user()->get(
            [],
            ['ID' => $userId]
        )->getUsers();

        if (empty($users)) {
            continue;
        }

        $user = $users[0];
        $userMap[$userId] = [
            'name' => $user->NAME,
            'lastName' => $user->LAST_NAME
        ];
    }

    // Step 7: Output results as a text table
    $table = [];

    // Header
    $table[] = [
        "Deal ID",
        "Name",
        "Responsible first name",
        "Responsible last name",
        "Expected revenue"
    ];

    // Data rows
    foreach ($deals as $deal) {
        $responsible = $userMap[$deal->assignedById] ?? ['name' => 'Unknown', 'lastName' => 'Unknown'];
        $table[] = [
            $deal->id,
            $deal->title,
            $responsible['name'],
            $responsible['lastName'],
            $deal->opportunity ?? 0
        ];
    }

    // Table output
    foreach ($table as $row) {
        echo implode("\t", $row) . "\n";
    }
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    funnel_name = input("Enter deal funnel name: ")

    try:
        categories = client.crm.category.list(entity_type_id=2).response.result.get("categories", [])
        selected_funnel = next(
            (c for c in categories if c.get("name") == funnel_name),
            None,
        )

        if not selected_funnel:
            print("Funnel not found.")
        else:
            stage_name = input("Enter stage name: ")
            funnel_id = int(selected_funnel["id"])
            entity_id = "DEAL_STAGE" if funnel_id == 0 else f"DEAL_STAGE_{funnel_id}"
            stages = client.crm.status.list(filter={"ENTITY_ID": entity_id}).response.result
            selected_stage = next(
                (s for s in stages if s.get("NAME") == stage_name),
                None,
            )

            if not selected_stage:
                print("Stage not found.")
            else:
                items = client.crm.item.list(
                    entity_type_id=2,
                    select=["id", "title", "assignedById", "opportunity"],
                    filter={"stageId": selected_stage["STATUS_ID"]},
                ).response.result.get("items", [])

                user_ids = sorted({int(item["assignedById"]) for item in items if item.get("assignedById")})
                users = client.user.get(filter={"ID": user_ids}).response.result if user_ids else []
                user_map = {
                    int(user["ID"]): {
                        "name": user.get("NAME", ""),
                        "lastName": user.get("LAST_NAME", ""),
                    }
                    for user in users
                }

                table = []

                table.append(
                    [
                        "Deal ID",
                        "Name",
                        "Responsible first name",
                        "Responsible last name",
                        "Expected revenue",
                    ]
                )

                for deal in items:
                    responsible = user_map.get(int(deal["assignedById"]), {"name": "Unknown", "lastName": "Unknown"})
                    table.append(
                        [
                            str(deal["id"]),
                            str(deal.get("title", "")),
                            str(responsible["name"]),
                            str(responsible["lastName"]),
                            str(deal.get("opportunity", 0)),
                        ]
                    )

                for row in table:
                    print("\t".join(row))
    except BitrixAPIError as error:
        print(f"Error: {error}")
    ```

{% endlist %}
