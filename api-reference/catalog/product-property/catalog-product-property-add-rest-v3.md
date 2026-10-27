# Add Product or Variation Property v 3.0 catalog.product.property.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" permission and permission to edit the catalog information block

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.property.add` method adds a string property of a product or variation to the trade catalog.

{% note warning "" %}

Only `id` and `controlId` are filled in the response; the other fields are returned as `null`. The saved property values can be retrieved using the [catalog.productProperty.get](./catalog-product-property-get.md) method.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object with the fields of the new property. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`integer`](../../data-types.md) | Identifier of the trade catalog to which the property is added. For a product property, pass the product catalog; for a variation property, pass the variation catalog.

The identifier can be retrieved using the [catalog.catalog.list](../catalog/catalog-catalog-list-rest-v3.md) method.

The method does not check that the information block is a trade catalog and adds the property to any existing information block ||
|| **name***
[`string`](../../data-types.md) | Property name. An empty string causes a validation error ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the property, for example `MATERIAL`. The method does not check the code for uniqueness ||
|| **sort**
[`integer`](../../data-types.md) | Sort order.

Default: `100` ||
|| **multiple**
[`string`](../../data-types.md) | Multiple property.

Possible values:

- `Y` — the property is multiple
- `N` — the property is single

Any other string value is saved as `N`.

Default: `N` ||
|| **isRequired**
[`string`](../../data-types.md) | Required property.

Possible values:

- `Y` — the property must be filled in on the product card
- `N` — the property is optional

Default: `N` ||
|#

The property type cannot be set: the method always creates a string property.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Material","code":"MATERIAL","sort":500,"multiple":"N","isRequired":"N"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Material","code":"MATERIAL","sort":500,"multiple":"N","isRequired":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductPropertyAddResult = {
      item: {
        id: number
        iblockId: number | null
        name: string | null
        sort: number | null
        code: string | null
        multiple: string | null
        isRequired: string | null
        controlId: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductPropertyAddResult>({
        method: 'catalog.product.property.add',
        params: {
          fields: {
            iblockId: 25,
            name: 'Material',
            code: 'MATERIAL',
            sort: 500,
            multiple: 'N',
            isRequired: 'N',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Property created:', result.item.id, result.item.controlId)
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
      async function addProductProperty() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.add',
            params: {
              fields: {
                iblockId: 25,
                name: 'Material',
                code: 'MATERIAL',
                sort: 500,
                multiple: 'N',
                isRequired: 'N',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Property created:', result.item.id, result.item.controlId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addProductProperty)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.property.add(
            fields={
                "iblockId": 25,
                "name": "Material",
                "code": "MATERIAL",
                "sort": 500,
                "multiple": "N",
                "isRequired": "N",
            },
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
                'catalog.product.property.add',
                [
                    'fields' => [
                        'iblockId' => 25,
                        'name' => 'Material',
                        'code' => 'MATERIAL',
                        'sort' => 500,
                        'multiple' => 'N',
                        'isRequired' => 'N',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.product.property.add',
        {
            fields: {
                iblockId: 25,
                name: 'Material',
                code: 'MATERIAL',
                sort: 500,
                multiple: 'N',
                isRequired: 'N',
            },
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
        'catalog.product.property.add',
        [
            'fields' => [
                'iblockId' => 25,
                'name' => 'Material',
                'code' => 'MATERIAL',
                'sort' => 500,
                'multiple' => 'N',
                'isRequired' => 'N',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.property.add", b24.Params{
    	"fields": b24.Params{
    		"iblockId":   25,
    		"name":       "Material",
    		"code":       "MATERIAL",
    		"sort":       500,
    		"multiple":   "N",
    		"isRequired": "N",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.property.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID        int    `json:"id"`
    	ControlID string `json:"controlId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.ControlID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 1231,
            "iblockId": null,
            "name": null,
            "sort": null,
            "code": null,
            "multiple": null,
            "isRequired": null,
            "controlId": "PROPERTY_1231"
        }
    },
    "time": {
        "start": 1792660313,
        "finish": 1792660313.118533,
        "duration": 0.11853289604187012,
        "processing": 0,
        "date_start": "2026-10-22T12:11:53+02:00",
        "date_finish": "2026-10-22T12:11:53+02:00",
        "operating_reset_at": 1792660860,
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
|| **result.item**
[`object`](../../data-types.md) | Fields of the created property. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the created property ||
|| **iblockId**
`null` | Information block identifier. Always `null` in the method response ||
|| **name**
`null` | Property name. Always `null` in the method response ||
|| **sort**
`null` | Sort order. Always `null` in the method response ||
|| **code**
`null` | Symbolic code. Always `null` in the method response ||
|| **multiple**
`null` | Multiple property flag. Always `null` in the method response ||
|| **isRequired**
`null` | Required property flag. Always `null` in the method response ||
|| **controlId**
[`string`](../../data-types.md) | Name of the property field in the information block forms in the `PROPERTY_<id>` format, for example `PROPERTY_1231` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Required field \"name\" is empty.",
                "field": "name"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `iblockId` | Required field `iblockId` is empty | Pass `iblockId` in `fields` ||
|| `name` | Required field `name` is empty | Pass `name` in `fields` ||
|| `id` | Field `id` is read-only | Remove `id` from `fields` ||
|| `controlId` | Field `controlId` is read-only | Remove `controlId` from `fields` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `iblockId` | This request requires data type `int` in field `iblockId` | Pass a number ||
|| `sort` | This request requires data type `int` in field `sort` | Pass an integer ||
|| `multiple` | This request requires data type `string` in field `multiple` | Pass the string `Y` or `N`, not `true` or `false` ||
|| `isRequired` | This request requires data type `string` in field `isRequired` | Pass the string `Y` or `N`, not `true` or `false` ||
|| `-` | Name is not specified.`<br>` | Pass a non-empty string in `name` ||
|| `200040300000` | Iblock Not Found | The information block was not found. Check the identifier using the [catalog.catalog.list](../catalog/catalog-catalog-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductPropertyDto` | Pass only `iblockId`, `name`, `code`, `sort`, `multiple`, `isRequired` in `fields`. The method does not accept the `propertyType` field ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" permission and permission to edit the catalog information block ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-field-list.md)
- [{#T}](./catalog-product-property-type-list.md)
- [{#T}](./catalog-product-property-get.md)
- [{#T}](./catalog-product-property-list.md)
