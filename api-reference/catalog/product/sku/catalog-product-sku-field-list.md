# Get List of Parent Product Fields v 3.0 catalog.product.sku.field.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `catalog.product.sku.field.list` returns the list of available parent product fields.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | Array of description properties to return in the response.

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
- `multiple` — whether the field is multiple
- `elementType` — element type for composite fields

Default: all description properties ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.sku.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","editableGroups"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","editableGroups"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.sku.field.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldListResult = {
      items: Array<{
        name?: string
        type?: string
        editableGroups?: string[] | null
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<FieldListResult>({
        method: 'catalog.product.sku.field.list',
        params: {
          select: ['name', 'type', 'editableGroups'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Fields:', result.items.length)
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
      async function listSkuFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.sku.field.list',
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
          console.info('Fields:', result.items.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listSkuFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.sku.field.list(
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
                'catalog.product.sku.field.list',
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
        'catalog.product.sku.field.list',
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
        'catalog.product.sku.field.list',
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
    res, err := client.Core().Call(ctx, "catalog.product.sku.field.list", b24.Params{
    	"select": []string{"name", "type", "editableGroups"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.sku.field.list: %w", err)
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
                "editableGroups": null
            },
            {
                "name": "iblockId",
                "type": "int",
                "editableGroups": [
                    "add"
                ]
            },
            {
                "name": "name",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "code",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "xmlId",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "active",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "sort",
                "type": "int",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "createdBy",
                "type": "int",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "modifiedBy",
                "type": "int",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "dateCreate",
                "type": "object",
                "editableGroups": null
            },
            {
                "name": "timestampX",
                "type": "object",
                "editableGroups": null
            },
            {
                "name": "dateActiveFrom",
                "type": "object",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "dateActiveTo",
                "type": "object",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "previewText",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "previewTextType",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "previewPicture",
                "type": "object",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "detailText",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "detailTextType",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "detailPicture",
                "type": "object",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "iblockSectionId",
                "type": "int",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "iblockSection",
                "type": "array",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "type",
                "type": "int",
                "editableGroups": null
            },
            {
                "name": "available",
                "type": "string",
                "editableGroups": null
            },
            {
                "name": "bundle",
                "type": "string",
                "editableGroups": null
            },
            {
                "name": "quantity",
                "type": "float",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "quantityReserved",
                "type": "float",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "quantityTrace",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "canBuyZero",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "subscribe",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "vatId",
                "type": "int",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "vatIncluded",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "purchasingPrice",
                "type": "float",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "purchasingCurrency",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "barcodeMulti",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "weight",
                "type": "float",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "length",
                "type": "float",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "width",
                "type": "float",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "height",
                "type": "float",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "measure",
                "type": "int",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "recurSchemeLength",
                "type": "int",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "recurSchemeType",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "trialPriceId",
                "type": "int",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "withoutOrder",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "quantityTraceRaw",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "paymentType",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "subscribeRaw",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "canBuyZeroRaw",
                "type": "string",
                "editableGroups": [
                    "add",
                    "update"
                ]
            },
            {
                "name": "properties",
                "type": "array",
                "editableGroups": [
                    "add",
                    "update"
                ]
            }
        ]
    },
    "time": {
        "start": 1792661181,
        "finish": 1792661181.509081,
        "duration": 0.5090808868408203,
        "processing": 0,
        "date_start": "2026-10-22T12:26:21+02:00",
        "date_finish": "2026-10-22T12:26:21+02:00",
        "operating_reset_at": 1792661760,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../../data-types.md) | Array of field descriptions. The description properties depend on `select`. [Element description](#items) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Field name ||
|| **type**
[`string`](../../../data-types.md) | Type in the metadata: `int`, `string`, `float`, `array`, `object`. For date and picture fields, `object` is returned ||
|| **title**
[`string`](../../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../../data-types.md) or `null` | Field description. For the fields of this object — `null` ||
|| **validationRules**
[`array`](../../../data-types.md) | Array of validation rules. For the fields of this object — `[]` ||
|| **requiredGroups**
[`array`](../../../data-types.md) or `null` | Array of operations in which the field is required. For `iblockId` and `name` — `["add"]`, for the others — `null` ||
|| **filterable**
[`boolean`](../../../data-types.md) | `true` — the field is available for filtering, `false` — not available ||
|| **sortable**
[`boolean`](../../../data-types.md) | `true` — the field is available for sorting, `false` — not available ||
|| **editable**
[`boolean`](../../../data-types.md) | `true` — the field can be passed in the operations from `editableGroups`, `false` — it cannot ||
|| **editableGroups**
[`array`](../../../data-types.md) or `null` | Array of operations in which the field can be set: `add`, `update`. For read-only fields — `null` ||
|| **multiple**
[`boolean`](../../../data-types.md) | Whether the field is multiple. For all fields — `false` ||
|| **elementType**
[`string`](../../../data-types.md) or `null` | Element type of a composite field. For all fields — `null` ||
|#

The method returns the descriptions of the fields: `id`, `iblockId`, `name`, `code`, `xmlId`, `active`, `sort`, `createdBy`, `modifiedBy`, `dateCreate`, `timestampX`, `dateActiveFrom`, `dateActiveTo`, `previewText`, `previewTextType`, `previewPicture`, `detailText`, `detailTextType`, `detailPicture`, `iblockSectionId`, `iblockSection`, `type`, `available`, `bundle`, `quantity`, `quantityReserved`, `quantityTrace`, `canBuyZero`, `subscribe`, `vatId`, `vatIncluded`, `purchasingPrice`, `purchasingCurrency`, `barcodeMulti`, `weight`, `length`, `width`, `height`, `measure`, `recurSchemeLength`, `recurSchemeType`, `trialPriceId`, `withoutOrder`, `quantityTraceRaw`, `paymentType`, `subscribeRaw`, `canBuyZeroRaw`, `properties`.

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

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | Pass only the properties from the `select` parameter description ||
|#

#### `select` Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions and the `catalog` scope ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-sku-add-rest-v3.md)
- [{#T}](./catalog-product-sku-update-rest-v3.md)
- [{#T}](./catalog-product-sku-get-rest-v3.md)
- [{#T}](./catalog-product-sku-list-rest-v3.md)
- [{#T}](./catalog-product-sku-delete-rest-v3.md)
- [{#T}](./catalog-product-sku-download-rest-v3.md)
- [{#T}](./catalog-product-sku-field-get.md)
