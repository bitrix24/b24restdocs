# How to Get Deal Pipelines with Stages and Semantics

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: any user with access to CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Deal pipelines help separate different sales processes: new sales, contract renewals, partner management, or individual business lines. Each pipeline has its own set of stages. Each stage has semantics—the state of the deal: in progress, won, or lost.

Semantics are required for reports, automation, and deal filtering. For example, they allow you to distinguish active deals from won and lost deals, even if the stage names differ across various pipelines.

As a result, you will obtain a table for each deal pipeline. The rows of the table will contain the stages, their names, and their semantics.

To retrieve deal pipelines with stages and semantics, call two methods sequentially:

1. [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — retrieve an array of `categories` containing deal pipelines and extract `id` and `name` from it
2. [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — for each pipeline, pass the stage codes into `filter.ENTITY_ID` to retrieve the stages

Data is passed between methods as follows:

- The `id` of the pipeline from `categories` determines the stage code
- For the main pipeline with `id = 0`, use code `DEAL_STAGE`
- For the additional pipeline with `id > 0`, use code `DEAL_STAGE_{id}`
- Pass the generated code into the `filter.ENTITY_ID` of the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method

## 1. Retrieve a List of Deal Pipelines

Call the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with the `entityTypeId: 2` parameter, where `2` is the identifier of the `deal` object type. CRM object type identifiers can be retrieved using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method.

The list of pipelines is filtered by user permissions. If a user does not have permission to read a specific pipeline, the method will not return it in the response.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    const categoryResponse = await $b24.actions.v2.call.make({
        method: 'crm.category.list',
        params: {
            entityTypeId: 2,
        },
        requestId: 'category-list',
    })

    if (!categoryResponse.isSuccess) {
        throw new Error(categoryResponse.getErrorMessages().join('; '))
    }

    const arCategory = categoryResponse.getData().result.categories.reduce((acc, item) => {
        acc[item.id] = item.name
        return acc
    }, {})
    ```

- PHP

    ```php
    // crm.category.list does not have a typed wrapper — calling via core
    $result = $sb->core->call('crm.category.list', ['entityTypeId' => 2])
        ->getResponseData()
        ->getResult();

    $arCategory = array_column($result['categories'] ?? [], 'name', 'id');
    ```

- Python

    ```python
    categories = client.crm.category.list(entity_type_id=2).response.result.get("categories", [])
    category_map = {item["id"]: item["name"] for item in categories}
    ```

- Go

    ```go
    // 1. The list of deal pipelines.
    res, err := client.Core().Call(ctx, "crm.category.list", b24.Params{
    	"entityTypeId": 2,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.category.list: %w", err)
    }

    // The method wraps the response in an object with the categories key.
    var categories struct {
    	Categories []struct {
    		ID   int    `json:"id"`
    		Name string `json:"name"`
    	} `json:"categories"`
    }
    if err := json.Unmarshal(res.Result, &categories); err != nil {
    	return fmt.Errorf("parse pipelines: %w", err)
    }
    ```

{% endlist %}

The method returns an array of `categories` in the response containing the deal pipelines available to the user, including the main pipeline. Each pipeline has an `id` (the pipeline identifier), a `name` (the name), and an `isDefault` (the flag indicating the main pipeline).

```json
{
    "result": {
        "categories": [
            {
                "id": 0,
                "name": "General",
                "sort": 100,
                "entityTypeId": 2,
                "isDefault": "Y"
            },
            {
                "id": 7,
                "name": "Contract renewal",
                "sort": 200,
                "entityTypeId": 2,
                "isDefault": "N"
            }
        ]
    },
    "total": 2
}
```

The `total` field shows the total number of pipelines found. The method returns a single page of results—up to 50 records. The examples above process the `items` received in the response.

## 2. Retrieve Stages and Semantics for Each Pipeline

The [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method retrieves stages using the `ENTITY_ID` filter. For deals, the stage code depends on the pipeline:

- `DEAL_STAGE` — stages of the main pipeline
- `DEAL_STAGE_{id}` — stages of an additional pipeline, where `{id}` is the pipeline identifier

Retrieve the `id` field from the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) response, form `ENTITY_ID`, and call [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) with sorting by `SORT`. For example, for a pipeline with ID `7`, you need to pass `DEAL_STAGE_7`.

Use the following fields in the response:

- `STATUS_ID` — stage identifier
- `NAME` — stage name
- `EXTRA.SEMANTICS` — stage semantics
- `EXTRA.COLOR` — stage color

The `EXTRA.SEMANTICS` value indicates the stage group:

- `process` — deal in progress
- `success` — deal won
- `failure` — deal lost
- `apology` — a separate group of lost stages

The examples below use data obtained in the previous step.

{% list tabs %}

- JS

    ```js
    for (const [id, name] of Object.entries(arCategory)) {
        const entityId = Number(id) > 0 ? `DEAL_STAGE_${id}` : 'DEAL_STAGE'

        const stageResponse = await $b24.actions.v2.call.make({
            method: 'crm.status.list',
            params: {
                order: { SORT: 'ASC' },
                filter: { ENTITY_ID: entityId },
            },
            requestId: `status-list-${id}`,
        })

        if (!stageResponse.isSuccess) {
            console.error(stageResponse.getErrorMessages().join('; '))
            continue
        }

        for (const item of stageResponse.getData().result) {
            console.log(name, item.STATUS_ID, item.NAME, item.EXTRA?.SEMANTICS)
        }
    }
    ```

- PHP

    ```php
    foreach ($arCategory as $id => $name)
    {
        $entityId = $id > 0 ? 'DEAL_STAGE_' . $id : 'DEAL_STAGE';

        $stages = $sb->getCRMScope()->status()->list(
            ['SORT' => 'ASC'],
            ['ENTITY_ID' => $entityId]
        )->getStatuses();

        foreach ($stages as $item)
        {
            echo $name . ': ' . $item->STATUS_ID . ': ' . $item->NAME
                . ' - ' . ($item->EXTRA['SEMANTICS'] ?? '') . PHP_EOL;
        }
    }
    ```

- Python

    ```python
    for category_id, category_name in category_map.items():
        entity_id = f"DEAL_STAGE_{category_id}" if int(category_id) > 0 else "DEAL_STAGE"
        result_deal = client.crm.status.list(
            order={"SORT": "ASC"},
            filter={"ENTITY_ID": entity_id},
        ).response.result

        for item in result_deal:
            print(
                category_name,
                item.get("STATUS_ID", ""),
                item.get("NAME", ""),
                (item.get("EXTRA") or {}).get("SEMANTICS", ""),
            )
    ```

- Go

    ```go
    // 2. The stages of each pipeline.
    for _, c := range categories.Categories {
    	// The default pipeline has stage IDs without a suffix, never DEAL_STAGE_0.
    	entityID := "DEAL_STAGE"
    	if c.ID > 0 {
    		entityID = fmt.Sprintf("DEAL_STAGE_%d", c.ID)
    	}

    	res, err := client.Core().Call(ctx, "crm.status.list", b24.Params{
    		"order":  b24.Params{"SORT": "ASC"},
    		"filter": b24.Params{"ENTITY_ID": entityID},
    	}, b24.WithIdempotent())
    	if err != nil {
    		// A pipeline the webhook is not allowed to read is skipped:
    		// the rest must not suffer because of it.
    		fmt.Fprintf(os.Stderr, "pipeline %d (%s): %v\n", c.ID, c.Name, err)
    		continue
    	}

    	var stages []stage
    	if err := json.Unmarshal(res.Result, &stages); err != nil {
    		return fmt.Errorf("parse the stages of pipeline %d: %w", c.ID, err)
    	}

    	for _, s := range stages {
    		fmt.Println(c.Name, s.StatusID, s.Name, s.Extra.Semantics)
    	}
    }
    ```

{% endlist %}

The method returns an array of stages for the specified `ENTITY_ID` in the response.

```json
{
    "result": [
        {
            "ENTITY_ID": "DEAL_STAGE_7",
            "STATUS_ID": "NEW",
            "NAME": "New",
            "EXTRA": {
                "SEMANTICS": "process",
                "COLOR": "#39A8EF"
            }
        },
        {
            "ENTITY_ID": "DEAL_STAGE_7",
            "STATUS_ID": "WON",
            "NAME": "Deal successful",
            "EXTRA": {
                "SEMANTICS": "success",
                "COLOR": "#7BD500"
            }
        },
        {
            "ENTITY_ID": "DEAL_STAGE_7",
            "STATUS_ID": "LOSE",
            "NAME": "Deal failed",
            "EXTRA": {
                "SEMANTICS": "failure",
                "COLOR": "#FF5752"
            }
        }
    ],
    "total": 3
}
```

The `total` field shows the total number of stages found for the specified `ENTITY_ID`. The method returns a single response page — up to 50 records. The examples above process the `items` received in the response.

## Full Code Example

The example outputs a table for each deal pipeline. The table shows the stage identifier, stage name, and semantics.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    const categoryResponse = await $b24.actions.v2.call.make({
        method: 'crm.category.list',
        params: {
            entityTypeId: 2,
        },
        requestId: 'category-list',
    })

    if (!categoryResponse.isSuccess) {
        throw new Error(categoryResponse.getErrorMessages().join('; '))
    }

    const arCategory = categoryResponse.getData().result.categories.reduce((acc, item) => {
        acc[item.id] = item.name
        return acc
    }, {})

    for (const [id, name] of Object.entries(arCategory)) {
        const entityId = Number(id) > 0 ? `DEAL_STAGE_${id}` : 'DEAL_STAGE'

        const stageResponse = await $b24.actions.v2.call.make({
            method: 'crm.status.list',
            params: {
                order: { SORT: 'ASC' },
                filter: { ENTITY_ID: entityId },
            },
            requestId: `status-list-${id}`,
        })

        if (!stageResponse.isSuccess) {
            console.error(stageResponse.getErrorMessages().join('; '))
            continue
        }

        const table = document.createElement('table')
        const caption = document.createElement('caption')
        caption.textContent = name
        table.appendChild(caption)

        const thead = document.createElement('thead')
        const trHead = document.createElement('tr')
        for (const text of ['STATUS ID', 'NAME', 'SEMANTICS']) {
            const th = document.createElement('th')
            th.textContent = text
            trHead.appendChild(th)
        }
        thead.appendChild(trHead)
        table.appendChild(thead)

        const tbody = document.createElement('tbody')
        for (const item of stageResponse.getData().result) {
            const tr = document.createElement('tr')
            if (item.EXTRA?.COLOR) {
                tr.style.color = item.EXTRA.COLOR
            }
            for (const value of [item.STATUS_ID, item.NAME, item.EXTRA?.SEMANTICS]) {
                const td = document.createElement('td')
                td.textContent = value ?? ''
                tr.appendChild(td)
            }
            tbody.appendChild(tr)
        }
        table.appendChild(tbody)

        document.body.appendChild(table)
    }

    $b24.destroy()
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // crm.category.list does not have a typed wrapper — calling via core
    $result = $sb->core->call('crm.category.list', ['entityTypeId' => 2])
        ->getResponseData()
        ->getResult();

    $arCategory = array_column($result['categories'] ?? [], 'name', 'id');

    foreach ($arCategory as $id => $name):
        $entityId = $id > 0 ? 'DEAL_STAGE_' . $id : 'DEAL_STAGE';

        $stages = $sb->getCRMScope()->status()->list(
            ['SORT' => 'ASC'],
            ['ENTITY_ID' => $entityId]
        )->getStatuses();

        if (!empty($stages)):
    ?>
            <table>
                <caption><?=htmlspecialchars((string)$name, ENT_QUOTES, 'UTF-8')?></caption>
                <thead>
                <tr>
                    <th>STATUS ID</th>
                    <th>NAME</th>
                    <th>SEMANTICS</th>
                </tr>
                </thead>
                <tbody>
                <?php foreach ($stages as $item): ?>
                    <?php
                    $statusId = htmlspecialchars((string)($item->STATUS_ID ?? ''), ENT_QUOTES, 'UTF-8');
                    $stageName = htmlspecialchars((string)($item->NAME ?? ''), ENT_QUOTES, 'UTF-8');
                    $semantics = htmlspecialchars((string)($item->EXTRA['SEMANTICS'] ?? ''), ENT_QUOTES, 'UTF-8');
                    $color = (string)($item->EXTRA['COLOR'] ?? '');
                    $colorStyle = preg_match('/^#[0-9A-Fa-f]{6}$/', $color) ? ' style="color:' . $color . '"' : '';
                    ?>
                <tr<?=$colorStyle?>>
                    <td><?=$statusId?></td>
                    <td><?=$stageName?></td>
                    <td><?=$semantics?></td>
                </tr>
                    <?php endforeach; ?>
                </tbody>
            </table>
        <?php endif; ?>
    <?php endforeach; ?>
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

    try:
        categories = client.crm.category.list(entity_type_id=2).response.result.get("categories", [])
        category_map = {item["id"]: item["name"] for item in categories}

        for category_id, category_name in category_map.items():
            entity_id = f"DEAL_STAGE_{category_id}" if int(category_id) > 0 else "DEAL_STAGE"
            result_deal = client.crm.status.list(
                order={"SORT": "ASC"},
                filter={"ENTITY_ID": entity_id},
            ).response.result

            print(category_name)
            print("STATUS ID\tNAME\tSEMANTICS")
            for item in result_deal:
                print(
                    "\t".join(
                        [
                            str(item.get("STATUS_ID", "")),
                            str(item.get("NAME", "")),
                            str((item.get("EXTRA") or {}).get("SEMANTICS", "")),
                        ]
                    )
                )
    except BitrixAPIError as error:
        print(error)
    ```

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //   go mod init example && go get github.com/bitrix24/b24gosdk
    // Run:
    //   export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    package main

    import (
    	"context"
    	"encoding/json"
    	"fmt"
    	"log"
    	"os"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    func main() {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	client := b24.NewClient(os.Getenv("B24_WEBHOOK_URL"))

    	// 1. The list of deal pipelines.
    	res, err := client.Core().Call(ctx, "crm.category.list", b24.Params{
    		"entityTypeId": 2,
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.category.list: %w", err)
    	}

    	// The method wraps the response in an object with the categories key.
    	var categories struct {
    		Categories []struct {
    			ID   int    `json:"id"`
    			Name string `json:"name"`
    		} `json:"categories"`
    	}
    	if err := json.Unmarshal(res.Result, &categories); err != nil {
    		return fmt.Errorf("parse pipelines: %w", err)
    	}

    	// 2. The stages of each pipeline.
    	for _, c := range categories.Categories {
    		// The default pipeline has stage IDs without a suffix, never DEAL_STAGE_0.
    		entityID := "DEAL_STAGE"
    		if c.ID > 0 {
    			entityID = fmt.Sprintf("DEAL_STAGE_%d", c.ID)
    		}

    		res, err := client.Core().Call(ctx, "crm.status.list", b24.Params{
    			"order":  b24.Params{"SORT": "ASC"},
    			"filter": b24.Params{"ENTITY_ID": entityID},
    		}, b24.WithIdempotent())
    		if err != nil {
    			// A pipeline the webhook is not allowed to read is skipped:
    			// the rest must not suffer because of it.
    			fmt.Fprintf(os.Stderr, "pipeline %d (%s): %v\n", c.ID, c.Name, err)
    			continue
    		}

    		var stages []stage
    		if err := json.Unmarshal(res.Result, &stages); err != nil {
    			return fmt.Errorf("parse the stages of pipeline %d: %w", c.ID, err)
    		}

    		for _, s := range stages {
    			fmt.Println(c.Name, s.StatusID, s.Name, s.Extra.Semantics)
    		}
    	}
    	return nil
    }

    // stage is a single row of the crm.status.list response.
    type stage struct {
    	StatusID string `json:"STATUS_ID"`
    	Name     string `json:"NAME"`

    	// The row also has a TOP-LEVEL SEMANTICS field, but for deal stages it
    	// arrives empty: the real value is in EXTRA. Reading the top-level field is
    	// the usual way to conclude that the portal has no semantics at all.
    	Extra struct {
    		Semantics string `json:"SEMANTICS"`
    	} `json:"EXTRA"`
    }
    ```

{% endlist %}

## If the Result Is Empty or an Error Occurs

If [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) or [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) returns an error, check the authorization and user permissions. To run the scenario, access to the CRM and the [`crm`](../../../api-reference/scopes/permissions.md) scope is required.

If [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) returns an empty array `categories`, the user cannot see the available deal pipelines. Check the user's CRM read permissions and repeat the scenario from the first step.

If [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) returns an empty array of stages, check the `ENTITY_ID` value:

- for the main pipeline with `id = 0`, pass `DEAL_STAGE`
- for an additional pipeline with `id > 0`, pass `DEAL_STAGE_{id}`
- do not pass `DEAL_STAGE_0`

After fixing `ENTITY_ID`, repeat the second step for this pipeline.

If the response contains the `total` field but not all `items` are processed, take the single response page limit into account. The examples in the tutorial only process the `items` received in the current response.

## Verify the Result

After running the example, tables containing the retrieved deal pipelines will appear on the page. The table heading is the pipeline name. The rows of the table display the stages of that pipeline:

- `STATUS ID` — the stage code, which can be used in deal fields and filters
- `NAME` — the stage name in the CRM interface
- `SEMANTICS` — the stage group: `process`, `success`, `failure`, or `apology`

If Bitrix24 contains only the main pipeline, the example will output one table. If additional pipelines have been created, there will be a separate table for each one.

The main pipeline has `id = 0`. Its stage code is `DEAL_STAGE`, without suffix `_0`.

## Continue Learning

- [{#T}](./how-to-get-stages-with-semantics.md)
- [{#T}](./how-to-get-elements-by-stage-filter.md)
- [{#T}](../../../api-reference/crm/status/crm-status-list.md)
