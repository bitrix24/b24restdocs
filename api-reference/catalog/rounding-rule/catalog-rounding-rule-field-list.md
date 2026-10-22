# Get List of Price Rounding Rule Fields v 3.0 catalog.roundingrule.field.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.roundingrule.field.list` returns the list of available price rounding rule fields.

{% note warning "" %}

In REST 3.0, the method group name is written in lowercase: `catalog.roundingrule.*`. Calling `catalog.roundingRule.*` at the `/rest/api/` address returns the `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION` error.

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | Array of strings with the names of the description properties to return for each field.

By default, all description properties are returned. The `["*"]` value is not supported.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required groups
- `filterable` — whether the field is available in the filter
- `sortable` — whether the field is available for sorting
- `editable` — whether the field is editable
- `editableGroups` — groups of operations in which the field is editable
- `multiple` — whether the field has multiple values
- `elementType` — element type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.roundingrule.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","editableGroups"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.roundingrule.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","editableGroups"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.roundingrule.field.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RoundingRuleFieldListResult = {
      items: Array<{
        name?: string
        type?: string
        editableGroups?: string[] | null
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<RoundingRuleFieldListResult>({
        method: 'catalog.roundingrule.field.list',
        params: {
          select: ['name', 'type', 'editableGroups'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Fields:', result.items)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.roundingrule.field.list',
            params: {
              select: ['name', 'type', 'editableGroups'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Fields:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.roundingrule.field.list(
            select=["name", "type", "editableGroups"],
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.roundingrule.field.list',
                [
                    'select' => ['name', 'type', 'editableGroups'],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.roundingrule.field.list',
        {
            select: ['name', 'type', 'editableGroups'],
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.roundingrule.field.list',
        [
            'select' => ['name', 'type', 'editableGroups'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.roundingrule.field.list", b24.Params{
    	"select": []string{"name", "type", "editableGroups"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.roundingrule.field.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		Name string `json:"name"`
    		Type string `json:"type"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.Name, item.Type)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "name": "id",
                "type": "int",
                "editableGroups": null
            },
            {
                "name": "catalogGroupId",
                "type": "int",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "price",
                "type": "float",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "roundType",
                "type": "int",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "roundPrecision",
                "type": "float",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "createdBy",
                "type": "int",
                "editableGroups": null
            },
            {
                "name": "dateCreate",
                "type": "object",
                "editableGroups": null
            },
            {
                "name": "modifiedBy",
                "type": "int",
                "editableGroups": null
            },
            {
                "name": "dateModify",
                "type": "object",
                "editableGroups": null
            }
        ]
    },
    "time": {
        "start": 1792668085,
        "finish": 1792668086.000152,
        "duration": 1.0001521110534668,
        "processing": 1,
        "date_start": "2026-10-22T14:21:25+02:00",
        "date_finish": "2026-10-22T14:21:26+02:00",
        "operating_reset_at": 1792668660,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | Array of price rounding rule field descriptions. [Element properties](#items) depend on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

The content of the description depends on `select`. The table lists all properties that can be requested.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field name: `id`, `catalogGroupId`, `price`, `roundType`, `roundPrecision`, `createdBy`, `dateCreate`, `modifiedBy`, `dateModify` ||
|| **type**
[`string`](../../data-types.md) | Type in the metadata: `int`, `float`, `object`. For `dateCreate` and `dateModify`, `object` is returned, although method responses pass the date as an ISO 8601 string ||
|| **title**
[`string`](../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../data-types.md) or `null` | Field description. For the fields of this object — `null` ||
|| **validationRules**
[`array`](../../data-types.md) | Validation rules. For the fields of this object — `[]`. The allowed values of `roundType` and `roundPrecision` are checked on write and are not passed in the metadata ||
|| **requiredGroups**
[`array`](../../data-types.md) or `null` | Array of strings with the names of operations in which the field is required. For `catalogGroupId`, `price`, `roundType`, `roundPrecision` — `["add"]`; for the other fields — `null` ||
|| **filterable**
[`boolean`](../../data-types.md) | `true` — the field is available for filtering, `false` — not available. For all fields — `true` ||
|| **sortable**
[`boolean`](../../data-types.md) | `true` — the field is available for sorting, `false` — not available. For all fields — `true` ||
|| **editable**
[`boolean`](../../data-types.md) | `true` — the field can be passed in the operations from `editableGroups`, `false` — it cannot. For `id`, `createdBy`, `dateCreate`, `modifiedBy`, `dateModify` — `false` ||
|| **editableGroups**
[`array`](../../data-types.md) or `null` | Array of strings with the names of operations in which the field can be set. For `catalogGroupId`, `price`, `roundType`, `roundPrecision` — `["add", "update"]`; for `id`, `createdBy`, `dateCreate`, `modifiedBy`, `dateModify` — `null` ||
|| **multiple**
[`boolean`](../../data-types.md) | Multiple value flag: `true` — multiple, `false` — single. For all fields — `false` ||
|| **elementType**
[`string`](../../data-types.md) or `null` | Element type of a composite field. For all fields — `null` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field \"foo\" found in entity \"DtoFieldDto\"."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | Pass only fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `editableGroups`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example, `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-rounding-rule-field-get.md)
- [{#T}](./catalog-rounding-rule-add-rest-v3.md)
- [{#T}](./catalog-rounding-rule-list-rest-v3.md)
