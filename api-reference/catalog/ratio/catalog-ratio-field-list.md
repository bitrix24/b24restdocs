# Get List of Ratio Fields v 3.0 catalog.ratio.field.list

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

The `catalog.ratio.field.list` method returns a list of available measurement unit ratio fields.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of strings with the names of description properties to return for each field.

Default: all description properties. The `["*"]` value is not supported.

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

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.ratio.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","filterable","sortable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.ratio.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","filterable","sortable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.ratio.field.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RatioFieldListResult = {
      items: Array<{
        name?: string
        type?: string
        filterable?: boolean
        sortable?: boolean
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<RatioFieldListResult>({
        method: 'catalog.ratio.field.list',
        params: {
          select: ['name', 'type', 'filterable', 'sortable'],
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
            method: 'catalog.ratio.field.list',
            params: {
              select: ['name', 'type', 'filterable', 'sortable'],
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
        bitrix_response = client.catalog.ratio.field.list(
            select=["name", "type", "filterable", "sortable"],
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
                'catalog.ratio.field.list',
                [
                    'select' => ['name', 'type', 'filterable', 'sortable'],
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
        'catalog.ratio.field.list',
        {
            select: ['name', 'type', 'filterable', 'sortable'],
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
        'catalog.ratio.field.list',
        [
            'select' => ['name', 'type', 'filterable', 'sortable'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.ratio.field.list", b24.Params{
    	"select": []string{"name", "type", "filterable", "sortable"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.ratio.field.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		Name string `json:"name"`
    		Type string `json:"type"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
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
                "filterable": true,
                "sortable": true
            },
            {
                "name": "productId",
                "type": "int",
                "filterable": true,
                "sortable": true
            },
            {
                "name": "ratio",
                "type": "float",
                "filterable": true,
                "sortable": true
            },
            {
                "name": "isDefault",
                "type": "bool",
                "filterable": true,
                "sortable": true
            }
        ]
    },
    "time": {
        "start": 1792668090,
        "finish": 1792668090.904361,
        "duration": 0.9043610095977783,
        "processing": 0,
        "date_start": "2026-10-22T14:21:30+02:00",
        "date_finish": "2026-10-22T14:21:30+02:00",
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
[`array`](../../data-types.md) | Array of measurement unit ratio field descriptions. The [element properties](#items) depend on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

The description depends on `select`. The table lists all properties that can be requested.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field name: `id`, `productId`, `ratio`, `isDefault` ||
|| **type**
[`string`](../../data-types.md) | Metadata type: `int`, `float`, `bool` ||
|| **title**
[`string`](../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../data-types.md) or `null` | Field description. `null` for the fields of this object ||
|| **validationRules**
[`array`](../../data-types.md) | Validation rules. For the fields of this object, `[]` ||
|| **requiredGroups**
[`array`](../../data-types.md) or `null` | Array of operation names for which the field is required. For all fields, `null` ||
|| **filterable**
[`boolean`](../../data-types.md) | `true` if filtering is supported; `false` otherwise. For all fields, `true` ||
|| **sortable**
[`boolean`](../../data-types.md) | `true` if sorting is supported; `false` otherwise. For all fields, `true` ||
|| **editable**
[`boolean`](../../data-types.md) | `true` if the field can be passed when writing; `false` otherwise. For all fields, `false` ||
|| **editableGroups**
[`array`](../../data-types.md) or `null` | Array of operation names in which the field can be set. For all fields, `null` ||
|| **multiple**
[`boolean`](../../data-types.md) | `true` for multiple values; `false` for a single value. For all fields, `false` ||
|| **elementType**
[`string`](../../data-types.md) or `null` | Type of an element in a composite field. For all fields, `null` ||
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

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

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
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-ratio-field-get.md)
- [{#T}](./catalog-ratio-get-rest-v3.md)
- [{#T}](./catalog-ratio-list-rest-v3.md)
