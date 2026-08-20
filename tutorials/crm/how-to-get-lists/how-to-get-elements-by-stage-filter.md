# How to Filter Items by Stage Name

> Scope: [`crm`, `user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — permission to read items of a CRM object
>
> - [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — a user with permission to read items of a CRM object
> - [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — any user
> - [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — a user with permission to read at least one CRM object
> - [user.get](../../../api-reference/user/user-get.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

The stage name is not stored in the CRM item. The "Stage" field holds an identifier of the form `C10:EXECUTING`, while the stage name is kept in a [directory](../../../api-reference/crm/status/index.md). That is why items cannot be filtered by the name directly: the stage identifier has to be retrieved first.

The stage identifier depends on the funnel, so the funnel is located by its name as well. As a result, we get a list of items at the required stage with the names of the responsible employees.

The scenario consists of four steps.

1. Retrieve the `id` of the funnel by its name using the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method
2. Retrieve the `STATUS_ID` of the stage by its name using the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method
3. Retrieve the items at that stage using the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method
4. Retrieve the names of the responsible users using the [user.get](../../../api-reference/user/user-get.md) method

## Before You Start

- The webhook is created on behalf of a user who has permission to read the items of the required CRM object. Steps 1 and 3 take their permissions into account: they will see only the funnels and items available to them

- The `crm` and `user_brief` scopes are selected in the webhook permissions

- You know the names of the funnel and the stage exactly as they are spelled in the interface: the examples compare the names strictly, including case and spaces

- The webhook URL grants full access within its scope. Retain the URL in an environment variable and never publish it in open code

The examples below use deals — `entityTypeId`: `2`. The identifiers `10` for the funnel and `C10:PREPAYMENT_INVOIC` for the stage are taken from one Bitrix24. In your Bitrix24 they will be different: each step substitutes the value from the response of the previous one.

## 1. Retrieve the Funnel Identifier

We will use the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with the following parameter:

- `entityTypeId` — the [object type](../../../api-reference/crm/data-types.md#object_type) identifier, a required parameter. We will specify `2` — a deal. To find out the `entityTypeId` of a smart process, execute the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method without parameters

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
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $logger = new Logger('b24');
    $logger->pushHandler(new StreamHandler('php://stdout'));

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $logger))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // crm.category.list has no wrapper in the SDK — we call the method directly
    $result = $serviceBuilder->core->call(
        'crm.category.list',
        [
            'entityTypeId' => 2 // 2 — a deal
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

As a result, we obtained the list of deal funnels. We will identify the required funnel by its name in the `name` field. The funnel identifier will be taken from the `id` field.

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
    "total": 4
}
```

## 2. Retrieve the Stage Identifier

We will use the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method with the filter:

- `ENTITY_ID` — the stage directory code. We will specify `DEAL_STAGE_10`, where `10` is the funnel identifier from step 1

How to assemble the directory code:

-  deals — `DEAL_STAGE_{id}`, where `{id}` is the funnel identifier

-  smart processes — `DYNAMIC_{entityTypeId}_STAGE_{id}`, for example `DYNAMIC_185_STAGE_11`, where `185` is the `entityTypeId` of the smart process and `11` is the funnel identifier

-  the default funnel with the identifier `0` — the code without the numeric part, `DEAL_STAGE`. There is no `DEAL_STAGE_0` code: with it, the method returns an empty list without an error

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

As a result, we obtained a list of stages. We will identify the required stage by its name in the `NAME` field. The stage identifier will be taken from the `STATUS_ID` field — it is exactly what goes into the filter in step 3.

Do not assemble `STATUS_ID` as a string in your own code. The value is limited to 21 characters, so in funnels with a two-digit identifier long codes get truncated: a stage with the standard code `PREPAYMENT_INVOICE` gets `C9:PREPAYMENT_INVOICE` in funnel `9`, but already `C10:PREPAYMENT_INVOIC` in funnel `10`, without the last letter. This does not affect the stage name: in the example below, the same stage is named "Approval".

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
            "CATEGORY_ID": "10",
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
            "CATEGORY_ID": "10",
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
            "STATUS_ID": "C10:PREPAYMENT_INVOIC",
            "NAME": "Approval",
            "NAME_INIT": "",
            "SORT": "30",
            "SYSTEM": "N",
            "CATEGORY_ID": "10",
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
            "CATEGORY_ID": "10",
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
            "CATEGORY_ID": "10",
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
            "CATEGORY_ID": "10",
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
            "CATEGORY_ID": "10",
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
            "CATEGORY_ID": "10",
            "COLOR": "#FF5752",
            "SEMANTICS": "F",
            "EXTRA": {
                "SEMANTICS": "apology",
                "COLOR": "#FF5752"
            }
        }
    ],
    "total": 8
}
```

## 3. Retrieve the List of Items at the Stage

Use the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method with the following parameters:

- `entityTypeId` — the [object type](../../../api-reference/crm/data-types.md#object_type) identifier, a required parameter. We will specify `2` — a deal. The value has to match the one passed in step 1

- `filter[stageId]` — the stage identifier from the `STATUS_ID` field of step 2. In the example, `C10:PREPAYMENT_INVOIC`. The filter accepts both a single value and an array of values, if items from several stages at once are needed

- `select` — the item fields to retrieve. Without this parameter, the method returns all fields, including custom ones, and the response becomes noticeably heavier

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
                "stageId": ["C10:PREPAYMENT_INVOIC"],
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
            "stageId" => ["C10:PREPAYMENT_INVOIC"],
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
            "stageId": ["C10:PREPAYMENT_INVOIC"],
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

As a result, we obtained a list of items at the requested stage. From the response, we take `assignedById` — the unique values of this field become the filter in step 4.

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
    "total": 5
}
```

## 4. Retrieve Responsible Person's Data

In the result of step 3, the responsible user is given as a number in the `assignedById` field. To display the first and last name, we will use the [user.get](../../../api-reference/user/user-get.md) method with the filter:

- `ID` — the `assignedById` values from step 3. We will collect the unique identifiers and pass them as an array: this way the data of all responsible users arrives in a single call instead of a separate call per item

{% list tabs %}

- JS
  
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: "user.get",
        params: {
            filter: { "ID": [1, 29] }
        }
    });
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getUserScope()->user()->get(
        [],
        ['ID' => [1, 29]]
    );
    ```

- Python

    ```python
    result = client.user.get(
        filter={"ID": [1, 29]},
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

As a result, we obtained the employee data, including the `NAME` and `LAST_NAME` fields.

```json
{
    "result": [
        {
            "ID": "1",
            "ACTIVE": true,
            "NAME": "Anna",
            "LAST_NAME": "Fischer",
            "SECOND_NAME": "",
            "EMAIL": "a.fischer@example.com",
            "WORK_POSITION": "Manager",
            "UF_DEPARTMENT": [1],
            "USER_TYPE": "employee"
        },
        {
            "ID": "29",
            "ACTIVE": true,
            "NAME": "Klaus",
            "LAST_NAME": "Weber",
            "SECOND_NAME": "",
            "EMAIL": "k.weber@example.com",
            "WORK_POSITION": "Manager",
            "UF_DEPARTMENT": [1],
            "USER_TYPE": "employee"
        }
    ],
    "total": 2
}
```

The response is abridged: the method also returns the remaining profile fields. The identifier comes as a string, while `assignedById` from step 3 comes as a number, so cast the values to the same type when matching an item with an employee.

## Verify the Result

The scenario is complete if the table has as many rows as there are items returned by step 3, and the name of the responsible user is filled in for every row.

What to check in the responses:

-  All items of step 3 are at the same stage. Add `stageId` to `select` and make sure the value matches the `STATUS_ID` from step 2

-  The `total` field of step 3 matches the stage counter in the kanban. Open the CRM → Deals section, switch to the funnel from step 1, and look at the column with the stage name from step 2

-  Every `assignedById` value from step 3 is present among the `ID` values from step 4. If an employee is not found, they have been dismissed or deleted — show such rows with the identifier instead of the name

An empty `items` array with non-empty steps 1 and 2 is not considered an error. What it means is described in the "Errors and Diagnostics" section.

## Errors and Diagnostics

If the method returned an error, check the request data.

#|
|| **Code** | **Cause and Action** ||
|| `NOT_FOUND` | In step 1 or 3, `entityTypeId` holds a value that matches no CRM object. A deal requires `2`; the identifier of a smart process is returned by the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | In step 1, a CRM object that has no funnels is passed. The stages of such an object are kept in a single directory without the numeric part ||
|| `400` `Invalid parameters.` | In step 2, the `filter` holds invalid values. The set of fields available for filtering is returned by the [crm.status.fields](../../../api-reference/crm/status/crm-status-fields.md) method ||
|| `400` `Access denied.` | In step 2 or 3, the webhook user has no permission to read the items of the CRM object. Check which user the webhook was created on behalf of ||
|| `INVALID_ARG_VALUE` `Invalid filter: field 'field' is not allowed in filter` | In step 3, the `filter` holds a field that cannot be filtered by. The list of available fields is returned by the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method ||
|#

More often, the scenario breaks not with an error but with an empty response. Analyze it step by step.

- An empty `categories` in step 1 — there is no funnel with such a name, or it is not available to the user. Call step 1 without searching by name and compare the spelling with the list

- An empty `result` in step 2 — the directory code is incorrect, check it against the rules from step 2. For a smart process, the code uses `entityTypeId`, not the `id` from the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method

- An empty `items` in step 3 with a non-empty step 2 — either the stage really has no items, or `stageId` was assembled as a string in the code and got truncated by length. Pass the value from the `STATUS_ID` field of the step 2 response

All four methods only read data, so after an error the scenario can be repeated from any step.

## Key Considerations

- Every funnel has its own stage directory. Stage names in different funnels may coincide, while `STATUS_ID` values do not, so a stage has to be retained together with the funnel identifier

- The names are compared by the example code, not by the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method: the method returns all the stages of the directory. The comparison is strict, so an extra space or a different letter case leaves the required stage unfound in the response

- The [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) and [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) methods return no more than 50 records per call. A funnel usually has fewer stages, but it may have more items — iterate over the pages with the `start` parameter

- Do not look for the stage semantics — success, failure, or in progress — in the `SEMANTICS` field: for stages in progress it comes empty, and the real value is kept in `EXTRA.SEMANTICS`. For details, see the [How to Retrieve a List of Stages with Semantics for CRM Entities](./how-to-get-stages-with-semantics.md) scenario

- To filter the items of another CRM object, replace `entityTypeId` in steps 1 and 3 and the directory code in step 2. The rest of the scenario logic does not change

## Code Example

The code goes through all four steps: it finds the identifiers of the funnel and the stage by their names and prints a table of items with the responsible users. The only thing to replace is the webhook URL in the environment variable.

The JS, PHP, and Python examples ask the user for the names, while in the Go example they are set by the `funnelName` and `stageName` constants. The Go example additionally creates a deal of its own at the selected stage and deletes it at the end, so that step 3 has something to find in an empty Bitrix24.

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

        // Ask for the funnel name
        let funnelName = await rl.question("Enter deal funnel name: ");

        // Step 1: Get the list of funnels and find the required one by name
        let categories = (await call("crm.category.list", { entityTypeId: 2 })).categories;
        let selectedFunnel = categories.find(cat => cat.name === funnelName);

        if (!selectedFunnel) {
            console.log("Funnel not found.");
            rl.close();
        } else {
            let funnelId = selectedFunnel.id;

            // Ask for the stage name
            let stageName = await rl.question("Enter stage name: ");
            rl.close();

            // Step 2: Get the funnel stages and find the required one by name
            let entityID = funnelId === 0 ? "DEAL_STAGE" : `DEAL_STAGE_${funnelId}`;

            let stages = await call("crm.status.list", { filter: { "ENTITY_ID": entityID } });
            let selectedStage = stages.find(stage => stage.NAME === stageName);

            if (!selectedStage) {
                console.log("Stage not found.");
            } else {
                let stageId = selectedStage.STATUS_ID;

                // Step 3: Get the deals at the selected stage
                let deals = (await call("crm.item.list", {
                    entityTypeId: 2,
                    select: ["id", "title", "assignedById", "opportunity"],
                    filter: {
                        "stageId": stageId,
                    },
                })).items;

                let uniqueResponsibleIds = [...new Set(deals.map(deal => deal.assignedById))];

                let userMap = {};

                // Step 4: Get information about the responsible users
                // One request for all of them at once, not one request per deal
                if (uniqueResponsibleIds.length > 0) {
                    let users = await call("user.get", { filter: { ID: uniqueResponsibleIds } });
                    users.forEach(user => {
                        userMap[Number(user.ID)] = {
                            name: user.NAME,
                            lastName: user.LAST_NAME
                        };
                    });
                }

                // Output the results to the console as a text table
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
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
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

    // Ask for the funnel name
    $funnelName = readline("Enter deal funnel name: ");

    // Step 1: Get the list of funnels and find the required one by name
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

    // Ask for the stage name
    $stageName = readline("Enter stage name: ");

    // Step 2: Get the funnel stages and find the required one by name
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

    // Step 3: Get the deals at the selected stage
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

    // Step 4: Get information about the responsible users
    // One request for all of them at once, not one request per deal
    if (!empty($uniqueResponsibleIds)) {
        $users = $serviceBuilder->getUserScope()->user()->get(
            [],
            ['ID' => array_values($uniqueResponsibleIds)]
        )->getUsers();

        foreach ($users as $user) {
            $userMap[(int)$user->ID] = [
                'name' => $user->NAME,
                'lastName' => $user->LAST_NAME
            ];
        }
    }

    // Output the results as a text table
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
    // its own deal, displays the list of items at the stage with the responsible persons, and cleans up after
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

## Continue Learning

- [{#T}](../../../api-reference/crm/universal/crm-item-list.md)
- [{#T}](../../../api-reference/crm/status/crm-status-list.md)
- [{#T}](../../../api-reference/crm/universal/category/crm-category-list.md)
- [{#T}](./how-to-get-stages-with-semantics.md)
- [{#T}](./how-to-get-deal-funnels.md)
- [{#T}](./get-activity-list-by-deals.md)
