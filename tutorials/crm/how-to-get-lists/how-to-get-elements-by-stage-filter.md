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

- Go

    ```go
    res, err := core.Call(ctx, "crm.category.list", b24.Params{
    	"entityTypeId": entityTypeDeal,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.category.list: %w", err)
    }

    // The method wraps the response in an object with the categories key.
    var categories struct {
    	Categories []struct {
    		ID        int    `json:"id"`
    		Name      string `json:"name"`
    		IsDefault string `json:"isDefault"`
    	} `json:"categories"`
    }
    if err := json.Unmarshal(res.Result, &categories); err != nil {
    	return fmt.Errorf("parse pipelines: %w", err)
    }

    // The required pipeline is identified by its title in the name field.
    funnel := -1
    for i, c := range categories.Categories {
    	if (funnelName == "" && c.IsDefault == "Y") || c.Name == funnelName {
    		funnel = i
    		break
    	}
    }
    if funnel < 0 {
    	return fmt.Errorf("pipeline %q not found", funnelName)
    }
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

- Go

    ```go
    // The default pipeline has stage IDs without a suffix, never
    // DEAL_STAGE_0. For a smart process the formula is different: DYNAMIC_185_STAGE_11.
    entityID := "DEAL_STAGE"
    if id := categories.Categories[funnel].ID; id > 0 {
    	entityID = fmt.Sprintf("DEAL_STAGE_%d", id)
    }

    res, err = core.Call(ctx, "crm.status.list", b24.Params{
    	"filter": b24.Params{"ENTITY_ID": entityID},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.status.list: %w", err)
    }

    var stages []struct {
    	StatusID string `json:"STATUS_ID"`
    	Name     string `json:"NAME"`
    	// The real semantics of the stage is in EXTRA: the top-level field
    	// SEMANTICS arrives empty for stages that are in progress.
    	Extra struct {
    		Semantics string `json:"SEMANTICS"`
    	} `json:"EXTRA"`
    }
    if err := json.Unmarshal(res.Result, &stages); err != nil {
    	return fmt.Errorf("parse stages: %w", err)
    }

    // The required stage is identified by its title in the NAME field, and the ID is taken
    // from STATUS_ID — it is exactly what goes into the filter of the next step.
    stage := -1
    for i, s := range stages {
    	if (stageName == "" && s.Extra.Semantics == "process") || s.Name == stageName {
    		stage = i
    		break
    	}
    }
    if stage < 0 {
    	return fmt.Errorf("stage %q not found in pipeline %s", stageName, entityID)
    }
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

- Go

    ```go
    res, err = core.Call(ctx, "crm.item.list", b24.Params{
    	"entityTypeId": entityTypeDeal,
    	"select":       []string{"id", "title", "assignedById", "opportunity"},
    	"filter":       b24.Params{"stageId": stages[stage].StatusID},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.item.list: %w", err)
    }

    // The method wraps the response in an object with the items key, and the fields here are in
    // camelCase — unlike crm.status.list two calls above.
    var list struct {
    	Items []struct {
    		ID           int     `json:"id"`
    		Title        string  `json:"title"`
    		AssignedByID int     `json:"assignedById"`
    		Opportunity  float64 `json:"opportunity"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &list); err != nil {
    	return fmt.Errorf("parse items: %w", err)
    }
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

- Go

    ```go
    res, err = core.Call(ctx, "user.get", b24.Params{
    	"filter": b24.Params{"ID": ids},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("user.get: %w", err)
    }

    // user.get responds in UPPER_SNAKE and sends the ID as a string.
    var rows []struct {
    	ID       b24.ID `json:"ID"`
    	Name     string `json:"NAME"`
    	LastName string `json:"LAST_NAME"`
    }
    if err := json.Unmarshal(res.Result, &rows); err != nil {
    	return fmt.Errorf("parse employees: %w", err)
    }
    for _, u := range rows {
    	users[int(u.ID)] = u.Name + " " + u.LastName
    }
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

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    //
    // The example is self-contained: it finds the pipeline and the stage, places at this stage
    // its own deal, displays the list of items at the stage with the responsible persons, and cleans up
    // itself. It runs on any portal, nothing needs to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"fmt"
    	"log"
    	"os"
    	"sort"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    // entityTypeDeal is the ID of the "deal" object type. The ID
    // of the smart process is returned by crm.enum.ownertype.
    const entityTypeDeal = 2

    // The titles of the pipeline and the stage being worked with. An empty string means
    // "choose on your own": the titles differ on every portal, and the example must
    // run everywhere without edits. Substitute your own here — the logic does not change.
    const (
    	funnelName = ""
    	stageName  = ""
    )

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- step 1: the pipeline ID
    	res, err := core.Call(ctx, "crm.category.list", b24.Params{
    		"entityTypeId": entityTypeDeal,
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.category.list: %w", err)
    	}

    	// The method wraps the response in an object with the categories key.
    	var categories struct {
    		Categories []struct {
    			ID        int    `json:"id"`
    			Name      string `json:"name"`
    			IsDefault string `json:"isDefault"`
    		} `json:"categories"`
    	}
    	if err := json.Unmarshal(res.Result, &categories); err != nil {
    		return fmt.Errorf("parse pipelines: %w", err)
    	}

    	// The required pipeline is identified by its title in the name field.
    	funnel := -1
    	for i, c := range categories.Categories {
    		if (funnelName == "" && c.IsDefault == "Y") || c.Name == funnelName {
    			funnel = i
    			break
    		}
    	}
    	if funnel < 0 {
    		return fmt.Errorf("pipeline %q not found", funnelName)
    	}
    	fmt.Printf("pipeline %q: id=%d\n", categories.Categories[funnel].Name, categories.Categories[funnel].ID)

    	// --- step 2: the stage ID
    	// The default pipeline has stage IDs without a suffix, never
    	// DEAL_STAGE_0. For a smart process the formula is different: DYNAMIC_185_STAGE_11.
    	entityID := "DEAL_STAGE"
    	if id := categories.Categories[funnel].ID; id > 0 {
    		entityID = fmt.Sprintf("DEAL_STAGE_%d", id)
    	}

    	res, err = core.Call(ctx, "crm.status.list", b24.Params{
    		"filter": b24.Params{"ENTITY_ID": entityID},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.status.list: %w", err)
    	}

    	var stages []struct {
    		StatusID string `json:"STATUS_ID"`
    		Name     string `json:"NAME"`
    		// The real semantics of the stage is in EXTRA: the top-level field
    		// SEMANTICS arrives empty for stages that are in progress.
    		Extra struct {
    			Semantics string `json:"SEMANTICS"`
    		} `json:"EXTRA"`
    	}
    	if err := json.Unmarshal(res.Result, &stages); err != nil {
    		return fmt.Errorf("parse stages: %w", err)
    	}

    	// The required stage is identified by its title in the NAME field, and the ID is taken
    	// from STATUS_ID — it is exactly what goes into the filter of the next step.
    	stage := -1
    	for i, s := range stages {
    		if (stageName == "" && s.Extra.Semantics == "process") || s.Name == stageName {
    			stage = i
    			break
    		}
    	}
    	if stage < 0 {
    		return fmt.Errorf("stage %q not found in pipeline %s", stageName, entityID)
    	}
    	fmt.Printf("stage %q: stageId=%s\n", stages[stage].Name, stages[stage].StatusID)

    	// --- setup: our own deal at this stage, so step 3 has something to find

    	dealID, err := addDeal(ctx, core, categories.Categories[funnel].ID, stages[stage].StatusID)
    	if err != nil {
    		return err
    	}
    	defer del(ctx, core, "crm.item.delete", b24.Params{
    		"entityTypeId": entityTypeDeal, "id": dealID,
    	})

    	// --- step 3: the items at the stage
    	res, err = core.Call(ctx, "crm.item.list", b24.Params{
    		"entityTypeId": entityTypeDeal,
    		"select":       []string{"id", "title", "assignedById", "opportunity"},
    		"filter":       b24.Params{"stageId": stages[stage].StatusID},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.item.list: %w", err)
    	}

    	// The method wraps the response in an object with the items key, and the fields here are in
    	// camelCase — unlike crm.status.list two calls above.
    	var list struct {
    		Items []struct {
    			ID           int     `json:"id"`
    			Title        string  `json:"title"`
    			AssignedByID int     `json:"assignedById"`
    			Opportunity  float64 `json:"opportunity"`
    		} `json:"items"`
    	}
    	if err := json.Unmarshal(res.Result, &list); err != nil {
    		return fmt.Errorf("parse items: %w", err)
    	}
    	fmt.Printf("items at the stage: %d\n", len(list.Items))

    	// --- step 4: the data of the responsible persons

    	// One request for all responsible persons rather than one request per item: the portal
    	// allows about two calls per second.
    	ids := make([]int, 0, len(list.Items))
    	seen := map[int]bool{}
    	for _, it := range list.Items {
    		if it.AssignedByID > 0 && !seen[it.AssignedByID] {
    			seen[it.AssignedByID] = true
    			ids = append(ids, it.AssignedByID)
    		}
    	}
    	sort.Ints(ids)

    	users := map[int]string{}
    	if len(ids) > 0 {
    		res, err = core.Call(ctx, "user.get", b24.Params{
    			"filter": b24.Params{"ID": ids},
    		}, b24.WithIdempotent())
    		if err != nil {
    			return fmt.Errorf("user.get: %w", err)
    		}

    		// user.get responds in UPPER_SNAKE and sends the ID as a string.
    		var rows []struct {
    			ID       b24.ID `json:"ID"`
    			Name     string `json:"NAME"`
    			LastName string `json:"LAST_NAME"`
    		}
    		if err := json.Unmarshal(res.Result, &rows); err != nil {
    			return fmt.Errorf("parse employees: %w", err)
    		}
    		for _, u := range rows {
    			users[int(u.ID)] = u.Name + " " + u.LastName
    		}
    	}

    	fmt.Println("Deal ID\tTitle\tResponsible\tExpected revenue")
    	for _, it := range list.Items {
    		who := users[it.AssignedByID]
    		if who == "" {
    			who = "Unknown"
    		}
    		fmt.Printf("%d\t%s\t%s\t%.0f\n", it.ID, it.Title, who, it.Opportunity)
    	}
    	return nil
    }

    // --- helpers: data setup and cleanup

    // addDeal places a deal at the chosen stage, so step 3 has something to find even
    // on an empty portal.
    func addDeal(ctx context.Context, core *b24.Core, categoryID int, stageID string) (b24.ID, error) {
    	res, err := core.Call(ctx, "crm.item.add", b24.Params{
    		"entityTypeId": entityTypeDeal,
    		"fields": b24.Params{
    			"title":       "Purchase of ovens",
    			"categoryId":  categoryID,
    			"stageId":     stageID,
    			"opportunity": 500,
    		},
    	})
    	if err != nil {
    		return 0, fmt.Errorf("crm.item.add: %w", err)
    	}
    	raw, ok := b24.Unwrap(res.Result, "item", "id")
    	if !ok {
    		return 0, fmt.Errorf("no item.id in %s", res.Result)
    	}
    	var id b24.ID
    	return id, json.Unmarshal(raw, &id)
    }

    // del removes what was created. A cleanup error is printed but not returned: it must not
    // mask the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, params b24.Params) {
    	if _, err := core.Call(ctx, method, params); err != nil {
    		fmt.Fprintf(os.Stderr, "cleanup, %s: %v
", method, err)
    	}
    }
    ```

{% endlist %}
