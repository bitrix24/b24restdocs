# Get Product Variation Fields by Filter catalog.product.offer.getFieldsByFilter

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" permission

The method `catalog.product.offer.getFieldsByFilter` returns the description of variation fields and properties for the specified variations catalog: type, whether the field is required, and whether it can be changed.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../../data-types.md) | Filter with the variations catalog identifier ([detailed description](#filter)) ||
|#

### Filter Parameter {#filter}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.iblockId`](../../data-types.md#catalog_catalog) | Identifier of the variations catalog information block.

You can retrieve the identifier using the [catalog.catalog.list](../../catalog/catalog-catalog-list.md) method: the variations catalog has the `productIblockId` field filled in ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"iblockId":27}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.getFieldsByFilter
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"iblockId":27},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.getFieldsByFilter
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
      userType?: string | null
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type OfferFieldsResult = {
      offer: Record<string, FieldDescription>
    }

    try {
      const response = await $b24.actions.v2.call.make<OfferFieldsResult>({
        method: 'catalog.product.offer.getFieldsByFilter',
        params: {
          filter: {
            iblockId: 27,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Offer fields:', Object.keys(result.offer))
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
      async function getOfferFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.offer.getFieldsByFilter',
            params: {
                  filter: {
                    iblockId: 27,
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
          console.info('Offer fields:', Object.keys(result.offer))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOfferFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.offer.get_fields_by_filter(
            filter={
                "iblockId": 27,
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
                'catalog.product.offer.getFieldsByFilter',
                [
                    'filter' => [
                        'iblockId' => 27,
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
        'catalog.product.offer.getFieldsByFilter',
        {
            filter: {
                iblockId: 27,
            },
        },
        function(result)
        {
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
        'catalog.product.offer.getFieldsByFilter',
        [
            'filter' => [
                'iblockId' => 27,
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
    res, err := client.Core().Call(ctx, "catalog.product.offer.getFieldsByFilter", b24.Params{
    	"filter": b24.Params{
    		"iblockId": 27,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.offer.getFieldsByFilter: %w", err)
    }

    // The method wraps the response in an object with the "offer" key.
    raw, ok := b24.Unwrap(res.Result, "offer")
    if !ok {
    	return fmt.Errorf("the response has no offer key")
    }

    // The key is the name of a field or a propertyN property, the value is the field description.
    var fields map[string]struct {
    	Type        string `json:"type"`
    	Name        string `json:"name"`
    	IsRequired  bool   `json:"isRequired"`
    	IsReadOnly  bool   `json:"isReadOnly"`
    	IsImmutable bool   `json:"isImmutable"`
    }
    if err := json.Unmarshal(raw, &fields); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for name, f := range fields {
    	fmt.Println(name, f.Type, f.IsRequired)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "offer": {
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
            "barcodeMulti": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Unique barcode for each item",
                "type": "char"
            },
            "bundle": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "In bundle",
                "type": "char"
            },
            "canBuyZero": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Make out-of-stock items available for purchase",
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
            "height": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Height",
                "type": "double"
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
            "length": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Length",
                "type": "double"
            },
            "measure": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Unit of measurement",
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
            "negativeAmountTrace": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "NEGATIVE_AMOUNT_TRACE",
                "type": "char"
            },
            "parentId": {
                "isDynamic": true,
                "isImmutable": false,
                "isMultiple": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Catalog Item",
                "propertyType": "E",
                "type": "productproperty",
                "userType": "SKU"
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
            "property429": {
                "isDynamic": true,
                "isImmutable": false,
                "isMultiple": true,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Pictures",
                "propertyType": "F",
                "type": "productproperty",
                "userType": ""
            },
            "property895": {
                "isDynamic": true,
                "isImmutable": false,
                "isMultiple": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Color",
                "propertyType": "S",
                "type": "productproperty",
                "userType": "directory"
            },
            "property907": {
                "isDynamic": true,
                "isImmutable": false,
                "isMultiple": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Clothing sizes",
                "propertyType": "L",
                "type": "productproperty",
                "userType": null
            },
            "property909": {
                "isDynamic": true,
                "isImmutable": false,
                "isMultiple": true,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Variation pictures",
                "propertyType": "F",
                "type": "productproperty",
                "userType": ""
            },
            "property925": {
                "isDynamic": true,
                "isImmutable": false,
                "isMultiple": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Date/Time",
                "propertyType": "S",
                "type": "productproperty",
                "userType": "DateTime"
            },
            "purchasingCurrency": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Purchase price currency",
                "type": "string"
            },
            "purchasingPrice": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Purchase price",
                "type": "string"
            },
            "quantity": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Quantity in stock",
                "type": "double"
            },
            "quantityReserved": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Quantity reserved",
                "type": "double"
            },
            "quantityTrace": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Stock control",
                "type": "char"
            },
            "recurSchemeLength": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Recurring payment time period",
                "type": "integer"
            },
            "recurSchemeType": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Recurring payment time unit",
                "type": "char"
            },
            "sort": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Sort index",
                "type": "integer"
            },
            "subscribe": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Enable product subscription",
                "type": "char"
            },
            "timestampX": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Modified on",
                "type": "datetime"
            },
            "trialPriceId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Product for trial payment",
                "type": "integer"
            },
            "type": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "name": "Product type",
                "type": "integer"
            },
            "vatId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Tax ID",
                "type": "integer"
            },
            "vatIncluded": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Tax included",
                "type": "char"
            },
            "weight": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Weight",
                "type": "double"
            },
            "width": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Width",
                "type": "double"
            },
            "withoutOrder": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "name": "Auto extend recurring subscription",
                "type": "char"
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
        "start": 1793891987,
        "finish": 1793891987.676673,
        "duration": 0.6766729354858398,
        "processing": 0,
        "date_start": "2026-11-05T18:19:47+01:00",
        "date_finish": "2026-11-05T18:19:47+01:00",
        "operating_reset_at": 1793892540,
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
|| **result.offer**
[`object`](../../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is a field of the [catalog_product_offer](../../data-types.md#catalog_product_offer) object, and `value` is an object of the [rest_field_description](../../data-types.md#rest_field_description) type.

Variation properties are returned with the `propertyN` key, where `N` is the property identifier, and with the `productproperty` type. For them, `propertyType` (the property type), `userType` (the custom type), and `isMultiple` (whether the property is multiple) are also returned. The property linking the variation to the parent product is returned with the `parentId` key ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "productType is not allowed for this catalog"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" permissions ||
|| `400` | `100` | Could not find value for parameter {filter} | The `filter` parameter was not provided ||
|| `400` | `0` | Incorrect filter format | `filter` is not an object ||
|| `400` | `0` | parameter - iblockId is empty | `iblockId` was not provided in `filter` ||
|| `400` | `0` | iblock is not catalog | The information block with the specified `iblockId` was not found or is not a catalog ||
|| `400` | `0` | productType is not allowed for this catalog | The product information block was passed in `iblockId` instead of the variations information block ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add.md)
- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-get.md)
- [{#T}](./catalog-product-offer-list.md)
- [{#T}](./catalog-product-offer-download.md)
- [{#T}](./catalog-product-offer-delete.md)
