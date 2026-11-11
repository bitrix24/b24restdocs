# Get Parent Product Fields by Filter catalog.product.sku.getFieldsByFilter

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission

The `catalog.product.sku.getFieldsByFilter` method returns the description of parent product fields and properties for the specified catalog information block: the type, whether the field is required, and whether it can be changed.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../../data-types.md) | Filter with the catalog information block identifier ([detailed description](#filter)) ||
|#

### Filter Parameter {#filter}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.id`](../../data-types.md#catalog_catalog) | Identifier of the product catalog information block.

You can retrieve the identifier using the [catalog.catalog.list](../../catalog/catalog-catalog-list.md) method: it is the catalog with `productIblockId` equal to `null`. The variation information block is not suitable ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"iblockId":25}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.getFieldsByFilter
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"iblockId":25},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.sku.getFieldsByFilter
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldDescription = {
      isImmutable: boolean
      isReadOnly: boolean
      isRequired: boolean
      name: string
      type: string
      isDynamic?: boolean
      isMultiple?: boolean
      propertyType?: string
      userType?: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SkuFieldsResult = {
      sku: Record<string, FieldDescription>
    }

    try {
      const response = await $b24.actions.v2.call.make<SkuFieldsResult>({
        method: 'catalog.product.sku.getFieldsByFilter',
        params: {
          filter: {
            iblockId: 25,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result.sku), result.sku)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getSkuFieldsByFilter() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.sku.getFieldsByFilter',
            params: {
              filter: {
                iblockId: 25,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result.sku), result.sku)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSkuFieldsByFilter)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.sku.get_fields_by_filter(
            filter={
                "iblockId": 25,
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.sku.getFieldsByFilter',
                [
                    'filter' => [
                        'iblockId' => 25,
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

    ```js
    BX24.callMethod(
        'catalog.product.sku.getFieldsByFilter',
        {
            filter: {
                iblockId: 25,
            }
        },
        function(result) {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.sku.getFieldsByFilter',
        [
            'filter' => [
                'iblockId' => 25,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.sku.getFieldsByFilter", b24.Params{
    	"filter": b24.Params{
    		"iblockId": 25,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.sku.getFieldsByFilter: %w", err)
    }

    // The method wraps the response in an object with the "sku" key.
    raw, ok := b24.Unwrap(res.Result, "sku")
    if !ok {
    	return fmt.Errorf("no sku key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "sku": {
            "active": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Active",
                "type": "char"
            },
            "available": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "Available for purchase",
                "type": "char"
            },
            "bundle": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "In bundle",
                "type": "char"
            },
            "code": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Symbolic code",
                "type": "string"
            },
            "createdBy": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Created by",
                "type": "integer"
            },
            "dateActiveFrom": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "DATE_ACTIVE_FROM",
                "type": "datetime"
            },
            "dateActiveTo": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "DATE_ACTIVE_TO",
                "type": "datetime"
            },
            "dateCreate": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Created on",
                "type": "datetime"
            },
            "detailPicture": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Full image",
                "type": "file"
            },
            "detailText": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Detailed description",
                "type": "string"
            },
            "detailTextType": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Detailed description type",
                "type": "string"
            },
            "iblockId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "name": "Information block ID",
                "type": "integer"
            },
            "iblockSection": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Sections",
                "type": "list"
            },
            "iblockSectionId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Main section",
                "type": "integer"
            },
            "id": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "ID",
                "type": "integer"
            },
            "modifiedBy": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Modified by",
                "type": "integer"
            },
            "name": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": true,
                "name": "Name",
                "type": "string"
            },
            "previewPicture": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Preview image",
                "type": "file"
            },
            "previewText": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Preview text",
                "type": "string"
            },
            "previewTextType": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Preview text type",
                "type": "string"
            },
            "priceType": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Payment type",
                "type": "char"
            },
            "property303": {
                "isDynamic": true,
                "isImmutable": false,
                "isMultiple": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Manufacturer",
                "propertyType": "S",
                "type": "productproperty",
                "userType": null
            },
            "property305": {
                "isDynamic": true,
                "isImmutable": false,
                "isMultiple": true,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Material",
                "propertyType": "S",
                "type": "productproperty",
                "userType": null
            },
            "property309": {
                "isDynamic": true,
                "isImmutable": false,
                "isMultiple": true,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Product Images",
                "propertyType": "F",
                "type": "productproperty",
                "userType": null
            },
            "sort": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Sort index",
                "type": "integer"
            },
            "timestampX": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Modified on",
                "type": "datetime"
            },
            "type": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "Product type",
                "type": "integer"
            },
            "xmlId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "External ID",
                "type": "string"
            }
        }
    },
    "time": {
        "start": 1793891795,
        "finish": 1793891795.746815,
        "duration": 0.7468149662017822,
        "processing": 0,
        "date_start": "2026-11-05T18:16:35+01:00",
        "date_finish": "2026-11-05T18:16:35+01:00",
        "operating_reset_at": 1793892360,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **result.sku**
[`object`](../../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` — identifier of the field of the [catalog_product_sku](../../data-types.md#catalog_product_sku) object or of the `propertyN` property, and `value` — object of type [rest_field_description](../../data-types.md#rest_field_description).

The method returns all properties of the information block. Properties have additional keys: `propertyType` — the property type, for example `S` — string, `L` — list, `F` — file, `userType` — the custom type, `isMultiple` — whether the property is multiple, `isDynamic` — always `true`. The example shows only the `property303`, `property305`, and `property309` properties.

The description of a parent product in cloud Bitrix24 does not include trade catalog fields such as `canBuyZero`, `vatId`, and `quantity`: the `add` and `update` methods do not retain them ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "parameter - iblockId is empty"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user does not have a permission to view the catalog ||
|| `400` | `100` | Could not find value for parameter {filter} | The `filter` parameter is not provided ||
|| `400` | `0` | Incorrect filter format | `filter` is not an object ||
|| `400` | `0` | parameter - iblockId is empty | `iblockId` is not provided in `filter` ||
|| `400` | `0` | iblock is not catalog | The `iblockId` information block is not a trade catalog ||
|| `400` | `0` | productType is not allowed for this catalog | `iblockId` contains the variation information block ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-sku-add.md)
- [{#T}](./catalog-product-sku-update.md)
- [{#T}](./catalog-product-sku-get.md)
- [{#T}](./catalog-product-sku-list.md)
- [{#T}](./catalog-product-sku-download.md)
- [{#T}](./catalog-product-sku-delete.md)