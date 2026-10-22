# Get List of Product Property Fields v 3.0 catalog.product.property.field.list

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

The `catalog.product.property.field.list` method returns the list of available product property fields.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of strings with the names of the description properties to return in the response.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required operation groups
- `filterable` — filtering availability flag
- `sortable` — sorting availability flag
- `editable` — editability flag
- `editableGroups` — operation groups in which the field is editable
- `multiple` — multiple value flag
- `elementType` — element type for composite fields

Default: all description properties. An empty array `[]` also returns all properties ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","requiredGroups","editableGroups"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","requiredGroups","editableGroups"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.field.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductPropertyFieldListResult = {
      items: Array<{
        name?: string
        type?: string
        requiredGroups?: string[] | null
        editableGroups?: string[] | null
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductPropertyFieldListResult>({
        method: 'catalog.product.property.field.list',
        params: {
          select: [
            'name',
            'type',
            'requiredGroups',
            'editableGroups',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(field => console.info(field.name, field.type))
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
      async function listProductPropertyFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.field.list',
            params: {
              select: [
                'name',
                'type',
                'requiredGroups',
                'editableGroups',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(field => console.info(field.name, field.type))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listProductPropertyFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.property.field.list(
            select=["name", "type", "requiredGroups", "editableGroups"],
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
                'catalog.product.property.field.list',
                [
                    'select' => ['name', 'type', 'requiredGroups', 'editableGroups'],
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
        'catalog.product.property.field.list',
        {
            select: ['name', 'type', 'requiredGroups', 'editableGroups'],
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
        'catalog.product.property.field.list',
        [
            'select' => ['name', 'type', 'requiredGroups', 'editableGroups'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.property.field.list", b24.Params{
    	"select": []string{"name", "type", "requiredGroups", "editableGroups"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.property.field.list: %w", err)
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
    for _, field := range result.Items {
    	fmt.Println(field.Name, field.Type)
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
                "requiredGroups": null,
                "editableGroups": null
            },
            {
                "name": "iblockId",
                "type": "int",
                "requiredGroups": [
                    "add"
                ],
                "editableGroups": [
                    "add"
                ]
            },
            {
                "name": "name",
                "type": "string",
                "requiredGroups": [
                    "add"
                ],
                "editableGroups": [
                    "add"
                ]
            },
            {
                "name": "sort",
                "type": "int",
                "requiredGroups": null,
                "editableGroups": [
                    "add"
                ]
            },
            {
                "name": "code",
                "type": "string",
                "requiredGroups": null,
                "editableGroups": [
                    "add"
                ]
            },
            {
                "name": "multiple",
                "type": "string",
                "requiredGroups": null,
                "editableGroups": [
                    "add"
                ]
            },
            {
                "name": "isRequired",
                "type": "string",
                "requiredGroups": null,
                "editableGroups": [
                    "add"
                ]
            },
            {
                "name": "controlId",
                "type": "string",
                "requiredGroups": null,
                "editableGroups": null
            }
        ]
    },
    "time": {
        "start": 1792660564,
        "finish": 1792660565.000883,
        "duration": 1.0008831024169922,
        "processing": 1,
        "date_start": "2026-10-22T12:16:04+02:00",
        "date_finish": "2026-10-22T12:16:05+02:00",
        "operating_reset_at": 1792661160,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | An array of field descriptions. The properties of each description depend on `select`. [Element description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

The table lists all properties that can be requested.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field name: `id`, `iblockId`, `name`, `sort`, `code`, `multiple`, `isRequired`, `controlId` ||
|| **type**
[`string`](../../data-types.md) | Type in the metadata: `int` or `string`. `string` for `multiple` and `isRequired`: values are passed as the strings `Y` and `N` ||
|| **title**
[`string`](../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../data-types.md) or `null` | Field description. `null` for property fields ||
|| **validationRules**
[`array`](../../data-types.md) | Validation rules. An empty array for property fields ||
|| **requiredGroups**
[`array`](../../data-types.md) or `null` | Operations in which the field is required. `["add"]` for `iblockId` and `name`; `null` for the other fields ||
|| **filterable**
[`boolean`](../../data-types.md) | `true` — the field is available for filtering. `false` for all fields ||
|| **sortable**
[`boolean`](../../data-types.md) | `true` — the field is available for sorting. `false` for all fields ||
|| **editable**
[`boolean`](../../data-types.md) | `true` — the field can be passed in the operations from `editableGroups`. `false` for `id` and `controlId` ||
|| **editableGroups**
[`array`](../../data-types.md) or `null` | Operations in which the field can be set. `["add"]` for `iblockId`, `name`, `sort`, `code`, `multiple`, and `isRequired`; `null` for `id` and `controlId` ||
|| **multiple**
[`boolean`](../../data-types.md) | Multiple value flag. `false` for all fields ||
|| **elementType**
[`string`](../../data-types.md) or `null` | Element type of a composite field. `null` for all fields ||
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

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | Specify in `select` only the properties from the `select` parameter description ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the app or webhook has the `catalog` scope ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-field-get.md)
- [{#T}](./catalog-product-property-add-rest-v3.md)
- [{#T}](./catalog-product-property-type-list.md)
