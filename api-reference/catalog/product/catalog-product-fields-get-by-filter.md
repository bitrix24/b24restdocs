# Get Product Fields by Filter v 3.0 catalog.product.fields.getbyfilter

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.product.fields.getbyfilter` returns the description of the fields of a simple product in the specified trade catalog, including the catalog properties.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../data-types.md) | Object with the catalog identifier. [Parameter description](#filter).

The method does not accept an array of conditions in the REST 3.0 format ||
|#

### Parameter filter {#filter}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`integer`](../../data-types.md) | Identifier of the product trade catalog. The key can also be passed as `IBLOCK_ID`.

The identifier can be retrieved using the [catalog.catalog.list](../catalog/catalog-catalog-list-rest-v3.md) method.

For an offers catalog, the method returns an error ||
|#

The method always returns the fields of a simple product. The product type from the filter is ignored.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.fields.getbyfilter`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"iblockId":25}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.fields.getbyfilter
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"iblockId":25},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.fields.getbyfilter
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductFieldsResult = {
      fields: Record<string, {
        TYPE: string
        IS_REQUIRED: boolean
        IS_READ_ONLY: boolean
        IS_IMMUTABLE: boolean
        NAME: string
        IS_DYNAMIC?: boolean
        IS_MULTIPLE?: boolean
        PROPERTY_TYPE?: string
        USER_TYPE?: string | null
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductFieldsResult>({
        method: 'catalog.product.fields.getbyfilter',
        params: {
          filter: {
            iblockId: 25,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Fields:', Object.keys(result.fields))
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
      async function getProductFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.fields.getbyfilter',
            params: {
              filter: {
                iblockId: 25,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Fields:', Object.keys(result.fields))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getProductFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.fields.getbyfilter(
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.fields.getbyfilter',
                [
                    'filter' => [
                        'iblockId' => 25,
                    ],
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
        'catalog.product.fields.getbyfilter',
        {
            filter: {
                iblockId: 25,
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
        'catalog.product.fields.getbyfilter',
        [
            'filter' => [
                'iblockId' => 25,
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
    res, err := client.Core().Call(ctx, "catalog.product.fields.getbyfilter", b24.Params{
    	"filter": b24.Params{
    		"iblockId": 25,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.fields.getbyfilter: %w", err)
    }

    var result struct {
    	Fields map[string]struct {
    		Type       string `json:"TYPE"`
    		IsRequired bool   `json:"IS_REQUIRED"`
    		Name       string `json:"NAME"`
    	} `json:"fields"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for code, field := range result.Fields {
    	fmt.Println(code, field.Type, field.IsRequired)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The method returns the description of all product fields and catalog properties. The example shows some of the fields.

```json
{
    "result": {
        "fields": {
            "ID": {
                "TYPE": "integer",
                "IS_REQUIRED": false,
                "IS_READ_ONLY": true,
                "IS_IMMUTABLE": false,
                "NAME": "ID"
            },
            "NAME": {
                "TYPE": "string",
                "IS_REQUIRED": true,
                "IS_READ_ONLY": false,
                "IS_IMMUTABLE": false,
                "NAME": "Name"
            },
            "IBLOCK_ID": {
                "TYPE": "integer",
                "IS_REQUIRED": true,
                "IS_READ_ONLY": false,
                "IS_IMMUTABLE": true,
                "NAME": "Information block ID"
            },
            "PREVIEW_PICTURE": {
                "TYPE": "file",
                "IS_REQUIRED": false,
                "IS_READ_ONLY": false,
                "IS_IMMUTABLE": false,
                "NAME": "Preview image"
            },
            "PROPERTY_301": {
                "TYPE": "productproperty",
                "IS_REQUIRED": false,
                "IS_READ_ONLY": false,
                "IS_IMMUTABLE": false,
                "NAME": "SKU code",
                "IS_DYNAMIC": true,
                "IS_MULTIPLE": false,
                "PROPERTY_TYPE": "S",
                "USER_TYPE": ""
            },
            "PROPERTY_305": {
                "TYPE": "productproperty",
                "IS_REQUIRED": false,
                "IS_READ_ONLY": false,
                "IS_IMMUTABLE": false,
                "NAME": "Material",
                "IS_DYNAMIC": true,
                "IS_MULTIPLE": true,
                "PROPERTY_TYPE": "S",
                "USER_TYPE": null
            },
            "TYPE": {
                "TYPE": "integer",
                "IS_REQUIRED": false,
                "IS_READ_ONLY": true,
                "IS_IMMUTABLE": false,
                "NAME": "Product type"
            },
            "QUANTITY": {
                "TYPE": "double",
                "IS_REQUIRED": false,
                "IS_READ_ONLY": false,
                "IS_IMMUTABLE": false,
                "NAME": "Quantity in stock"
            },
            "VAT_ID": {
                "TYPE": "integer",
                "IS_REQUIRED": false,
                "IS_READ_ONLY": false,
                "IS_IMMUTABLE": false,
                "NAME": "Tax ID"
            },
            "MEASURE": {
                "TYPE": "integer",
                "IS_REQUIRED": false,
                "IS_READ_ONLY": false,
                "IS_IMMUTABLE": false,
                "NAME": "Unit of measurement"
            }
        }
    },
    "time": {
        "start": 1792660854,
        "finish": 1792660854.262779,
        "duration": 0.26277899742126465,
        "processing": 0,
        "date_start": "2026-10-22T12:20:54+02:00",
        "date_finish": "2026-10-22T12:20:54+02:00",
        "operating_reset_at": 1792661400,
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
|| **result.fields**
[`object`](../../data-types.md) | Field descriptions. The key is the field code in uppercase, for example `IBLOCK_ID`, and for a property — `PROPERTY_{id}`. [Value description](#field) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Field Description {#field}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE**
[`string`](../../data-types.md) | Field type: `integer`, `double`, `string`, `char`, `datetime`, `file`, `list`. For properties — `productproperty` ||
|| **IS_REQUIRED**
[`boolean`](../../data-types.md) | `true` — the field is required when creating a product ||
|| **IS_READ_ONLY**
[`boolean`](../../data-types.md) | `true` — the field is read-only ||
|| **IS_IMMUTABLE**
[`boolean`](../../data-types.md) | `true` — the field can be set only when creating a product ||
|| **NAME**
[`string`](../../data-types.md) | Field name ||
|| **IS_DYNAMIC**
[`boolean`](../../data-types.md) | Properties only. `true` — the field is a catalog property ||
|| **IS_MULTIPLE**
[`boolean`](../../data-types.md) | Properties only. `true` — the property is multiple ||
|| **PROPERTY_TYPE**
[`string`](../../data-types.md) | Properties only. Basic type of the property:

- `S` — string
- `N` — number
- `L` — list
- `F` — file
- `E` — binding to elements
- `G` — binding to sections ||
|| **USER_TYPE**
[`string`](../../data-types.md) | Properties only. User type of the property, for example `directory` or `Date`. `null` or an empty string if the type is not set ||
|#

Field codes are returned in the format of the previous API version, and their set does not match the REST 3.0 fields. Product fields for REST 3.0 methods are returned by [catalog.product.field.list](./catalog-product-field-list.md). Pass the `PROPERTY_{id}` property keys from the response in the `properties` field of the [catalog.product.add](./catalog-product-add-rest-v3.md) and [catalog.product.update](./catalog-product-update-rest-v3.md) methods.

In the previous API version, each product type has its own method: [catalog.product.getFieldsByFilter](./catalog-product-get-fields-by-filter.md), [catalog.product.sku.getFieldsByFilter](./sku/catalog-product-sku-get-fields-by-filter.md), [catalog.product.offer.getFieldsByFilter](./offer/catalog-product-offer-get-fields-by-filter.md), and [catalog.product.service.getFieldsByFilter](./service/catalog-product-service-get-fields-by-filter.md). They return fields in an object named after the type, for example `result.product` or `result.service`, with keys in the `iblockId` format and the `isRequired`, `isReadOnly`, and `isImmutable` flags. In REST 3.0, there is one method, and it returns only the fields of a simple product.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "productType is not allowed for this catalog"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Required field `filter` is missing | Pass the `filter` object with the catalog identifier ||
|| `-` | Incorrect filter format | Pass the `iblockId` key in `filter` ||
|| `-` | parameter - iblockId is empty | Pass `filter` as an object like `{"iblockId": 25}` with a numeric value ||
|| `-` | iblock is not catalog | The information block is not a trade catalog. Pass the trade catalog identifier ||
|| `-` | productType is not allowed for this catalog | Pass the identifier of the product catalog, not the offers catalog ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions to view the catalog ||
|#

#### Method Name Error

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `catalog.product.fields.getByFilter` was not found | Call the method in lowercase: `catalog.product.fields.getbyfilter` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-field-list.md)
- [{#T}](./catalog-product-add-rest-v3.md)
- [{#T}](./catalog-product-get-fields-by-filter.md)
