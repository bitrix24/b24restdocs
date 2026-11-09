# Get Product Fields by Filter catalog.product.getFieldsByFilter

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" permission

The `catalog.product.getFieldsByFilter` method retrieves the descriptions of simple product fields in the specified catalog, including catalog properties.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../data-types.md) | An object with the catalog identifier [(detailed description)](#filter) ||
|#

### Filter Parameter {#filter}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the product trade catalog. You can retrieve the identifier using the [catalog.catalog.list](../catalog/catalog-catalog-list.md) method.

For a variation catalog, the method returns the `productType is not allowed for this catalog` error ||
|#

The method always returns the fields of a simple product. The `productType` key in `filter` is ignored.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"iblockId":25}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.getFieldsByFilter
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"iblockId":25},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.getFieldsByFilter
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldItem = {
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
    type ProductFieldsByFilterResult = {
      product: Record<string, FieldItem>
    }

    try {
      const response = await $b24.actions.v2.call.make<ProductFieldsByFilterResult>({
        method: 'catalog.product.getFieldsByFilter',
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
        console.info(result.product)
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
      async function getProductFieldsByFilter() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.getFieldsByFilter',
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
          console.info(result.product)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getProductFieldsByFilter)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.get_fields_by_filter(
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
                'catalog.product.getFieldsByFilter',
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
        'catalog.product.getFieldsByFilter',
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
        'catalog.product.getFieldsByFilter',
        [
            'filter' => [
                'iblockId' => 25
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
    res, err := client.Core().Call(ctx, "catalog.product.getFieldsByFilter", b24.Params{
    	"filter": b24.Params{
    		"iblockId": 25,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.getFieldsByFilter: %w", err)
    }

    // The method wraps the response in an object with the "product" key.
    raw, ok := b24.Unwrap(res.Result, "product")
    if !ok {
    	return fmt.Errorf("no product key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "product": {
            "active": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Activity",
                "type": "char"
            },
            "available": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "Availability for Purchase",
                "type": "char"
            },
            "bundle": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "Set Availability",
                "type": "char"
            },
            "code": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Symbolic Code",
                "type": "string"
            },
            "dateActiveFrom": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "DATE_ACTIVE_FROM",
                "type": "datetime"
            },
            "dateCreate": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Creation Date",
                "type": "datetime"
            },
            "detailPicture": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Detail Picture",
                "type": "file"
            },
            "iblockId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "name": "Information Block Identifier",
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
                "name": "Main Section",
                "type": "integer"
            },
            "id": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "Identifier",
                "type": "integer"
            },
            "measure": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Unit of Measurement",
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
                "name": "Preview Picture",
                "type": "file"
            },
            "property301": {
                "isDynamic": true,
                "isImmutable": false,
                "isMultiple": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Article number",
                "propertyType": "S",
                "type": "productproperty",
                "userType": ""
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
            "purchasingCurrency": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Purchasing Price Currency",
                "type": "string"
            },
            "purchasingPrice": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Purchasing Price",
                "type": "string"
            },
            "quantity": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Available Quantity",
                "type": "double"
            },
            "type": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "Product Type",
                "type": "integer"
            },
            "vatId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "VAT Identifier",
                "type": "integer"
            },
            "vatIncluded": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "VAT Included in Price",
                "type": "char"
            },
            "xmlId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "External Code",
                "type": "string"
            }
        }
    },
    "time": {
        "start": 1793892123,
        "finish": 1793892123.226906,
        "duration": 0.22690606117248535,
        "processing": 0,
        "date_start": "2026-11-05T18:22:03+01:00",
        "date_finish": "2026-11-05T18:22:03+01:00",
        "operating_reset_at": 1793892720,
        "operating": 0
    }
}
```

The method returns the descriptions of all product fields and catalog properties. The example shows some of the fields.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.product**
[`object`](../../data-types.md) | An object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of a [catalog_product](../data-types.md#catalog_product) object field, and `value` is an object of type [rest_field_description](../data-types.md#rest_field_description).

Catalog properties are returned with the `property{id}` key, for example `property301`. Their description contains additional fields:

- `isDynamic` — `true`, the field is a catalog property
- `isMultiple` — `true` if the property is multiple
- `propertyType` — the base property type: `S` — string, `N` — number, `L` — list, `F` — file, `E` — link to elements, `G` — link to sections
- `userType` — the custom property type, for example `directory` or `Date`. If the type is not set — `null` or an empty string ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "200040300010",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Cause** ||
|| `400` | `200040300010` | `Access Denied` | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" permissions ||
|| `400` | `100` | `Could not find value for parameter {filter}` | The required `filter` parameter was not provided ||
|| `400` | `0` | `Incorrect filter format` | The `filter` parameter was not passed as an object, for example as a string ||
|| `400` | `0` | `parameter - iblockId is empty` | The `iblockId` key was not passed in `filter`, or its value is not a number ||
|| `400` | `0` | `iblock is not catalog` | The information block with the specified `iblockId` does not exist or is not a trade catalog ||
|| `400` | `0` | `productType is not allowed for this catalog` | A variation catalog identifier was passed. Pass the identifier of the product catalog ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-add.md)
- [{#T}](./catalog-product-update.md)
- [{#T}](./catalog-product-get.md)
- [{#T}](./catalog-product-list.md)
- [{#T}](./catalog-product-download.md)
- [{#T}](./catalog-product-delete.md)
- [{#T}](./catalog-product-fields-get-by-filter.md)
