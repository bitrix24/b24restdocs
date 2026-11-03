# Add a Product Item v 3.0 crm.[type].productrow.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM item to which the product item is added

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.productrow.add` method adds one product item to a CRM item and returns its ID. The object type is set by the `<type>` segment of the method name. The item amount is recalculated from the product items unless it was set manually.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.productrow.add` ||
|| Lead | `crm.lead.productrow.add` ||
|| Estimate | `crm.quote.productrow.add` ||
|| Invoice | `crm.smartinvoice.productrow.add` ||
|| CRM document | `crm.smartdocument.productrow.add` ||
|| SPA | `crm.smartprocess<entityTypeId>.productrow.add`, for example, `crm.smartprocess1038.productrow.add` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. An SPA has product item methods only if linking to catalog products is enabled in the type settings.

The method name is written in lowercase: `crm.smartinvoice.productrow.add`, not `crm.smartInvoice.productRow.add`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the product item fields in the format `{"field": "value"}`. [Parameter description](#fields)

The required field is `ownerId`, and the object must contain at least one other field ||
|#

### The fields Parameter {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerId***
[`integer`](../../../data-types.md) | ID of the CRM item to which the product item is added. The item type is set by the method name: `crm.deal.productrow.add` adds a product item to a deal.

The ID can be retrieved with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method or from the response of the [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md) method ||
|| **productId**
[`integer`](../../../data-types.md) | ID of a catalog product. If `productName` is not passed, the product item name is taken from the catalog.

The ID can be retrieved with the [catalog.product.list](../../../catalog/product/catalog-product-list-rest-v3.md) method.

If the field is not passed, the product item is created without a link to the catalog, and responses contain `productId: 0` ||
|| **productName**
[`string`](../../../data-types.md) | Product item name. For a product item without a catalog product, pass the name explicitly ||
|| **price**
[`double`](../../../data-types.md) | Unit price including discount and tax ||
|| **quantity**
[`double`](../../../data-types.md) | Quantity ||
|| **discountTypeId**
[`integer`](../../../data-types.md) | Discount type.

Possible values:

- `1` — fixed amount discount, set in `discountSum`
- `2` — percentage discount, set in `discountRate`

Default: `2` ||
|| **discountRate**
[`double`](../../../data-types.md) | Discount percentage. Use if `discountTypeId` equals `2` ||
|| **discountSum**
[`double`](../../../data-types.md) | Discount amount per unit. Use if `discountTypeId` equals `1`. With a percentage discount, the amount is calculated automatically ||
|| **taxRate**
[`double`](../../../data-types.md) | Tax rate in percent, for example, `20`. Without a value, the product item has no tax.

Default: `null` ||
|| **taxIncluded**
[`boolean`](../../../data-types.md) | Whether the tax is included in the price.

Possible values:

- `true` — the tax is included in `price`
- `false` — the tax is added on top of the price

Default: `false` ||
|| **measureCode**
[`integer`](../../../data-types.md) | Unit of measure code from the classifier, for example, `796` — piece. The unit name in `measureName` is set by the code.

Unit of measure codes can be retrieved with the [catalog.measure.list](../../../catalog/measure/catalog-measure-list-rest-v3.md) method ||
|| **measureName**
[`string`](../../../data-types.md) | Unit of measure name. If `measureCode` is passed, the name is taken from the units of measure reference ||
|| **sort**
[`integer`](../../../data-types.md) | Position of the product item in the list.

Default: `0` ||
|#

The `id`, `ownerTypeId`, and `productTypeId` fields are read-only: the method returns an error for them. The list of fields, with an indication of whether each field can be edited, is returned by the `crm.<type>.productrow.field.list` method. The fields are described in the [product item methods overview](./index.md#fields).

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.productrow.add`

{% endnote %}

The examples add a catalog product to an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ownerId":37,"productId":6461,"price":100.5,"quantity":2,"discountTypeId":2,"discountRate":10,"taxRate":20,"taxIncluded":true,"measureCode":796}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.productrow.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ownerId":37,"productId":6461,"price":100.5,"quantity":2,"discountTypeId":2,"discountRate":10,"taxRate":20,"taxIncluded":true,"measureCode":796},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.productrow.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductRowAddResult = {
      id: number
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ProductRowAddResult>(
        'crm.smartprocess1038.productrow.add',
        {
          fields: {
            ownerId: 37,
            productId: 6461,
            price: 100.5,
            quantity: 2,
            discountTypeId: 2,
            discountRate: 10,
            taxRate: 20,
            taxIncluded: true,
            measureCode: 796,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Product row created:', result.id)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addProductRow() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.productrow.add',
            {
              fields: {
                ownerId: 37,
                productId: 6461,
                price: 100.5,
                quantity: 2,
                discountTypeId: 2,
                discountRate: 10,
                taxRate: 20,
                taxIncluded: true,
                measureCode: 796,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Product row created:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addProductRow)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.productrow.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "ownerId": 37,
                    "productId": 6461,
                    "price": 100.5,
                    "quantity": 2,
                    "discountTypeId": 2,
                    "discountRate": 10,
                    "taxRate": 20,
                    "taxIncluded": True,
                    "measureCode": 796,
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.productrow.add',
                [
                    'fields' => [
                        'ownerId' => 37,
                        'productId' => 6461,
                        'price' => 100.5,
                        'quantity' => 2,
                        'discountTypeId' => 2,
                        'discountRate' => 10,
                        'taxRate' => 20,
                        'taxIncluded' => true,
                        'measureCode' => 796,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.productrow.add',
        {
            fields: {
                ownerId: 37,
                productId: 6461,
                price: 100.5,
                quantity: 2,
                discountTypeId: 2,
                discountRate: 10,
                taxRate: 20,
                taxIncluded: true,
                measureCode: 796,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.productrow.add',
        [
            'fields' => [
                'ownerId' => 37,
                'productId' => 6461,
                'price' => 100.5,
                'quantity' => 2,
                'discountTypeId' => 2,
                'discountRate' => 10,
                'taxRate' => 20,
                'taxIncluded' => true,
                'measureCode' => 796,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.productrow.add", b24.Params{
    	"fields": b24.Params{
    		"ownerId":        37,
    		"productId":      6461,
    		"price":          100.5,
    		"quantity":       2,
    		"discountTypeId": 2,
    		"discountRate":   10,
    		"taxRate":        20,
    		"taxIncluded":    true,
    		"measureCode":    796,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.productrow.add: %w", err)
    }

    var result struct {
    	ID int `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 2409
    },
    "time": {
        "start": 1793709949,
        "finish": 1793709949.853585,
        "duration": 0.8535850048065186,
        "processing": 0,
        "date_start": "2026-11-03T15:45:49+03:00",
        "date_finish": "2026-11-03T15:45:49+03:00",
        "operating_reset_at": 1793710260,
        "operating": 0.3442521095275879
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.id**
[`integer`](../../../data-types.md) | ID of the created product item. The product item fields can be retrieved with the [crm.&lt;type&gt;.productrow.get](./crm-entity-product-row-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
                "message": "Field value is not one of the possible values.",
                "field": "productId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `-` | No product row fields specified | Pass at least one field other than `ownerId` in `fields`, for example, `productId` or `productName` ||
|| `price` | This request requires data type `float` in field `price` | Pass a value of the type specified in the field description ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `ownerId` | Required field `ownerId` is empty | Pass `ownerId` in `fields` ||
|| `ownerId` | Field value must be 1 or greater | Pass a positive item ID ||
|| `ownerId` | Field value is not one of the possible values | This object type has no item with this ID, or you do not have access to it. Check `ownerId` and the method name ||
|| `productId` | Field value is not one of the possible values | The product is not in the catalog, or you do not have access to it. Check `productId` with the [catalog.product.list](../../../catalog/product/catalog-product-list-rest-v3.md) method ||
|| `ownerTypeId` | Field `ownerTypeId` is read-only | Remove the read-only field from `fields`. The method responds the same way to `id` and `productTypeId` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductRowDto` | Check the field name against the [fields parameter](#fields) table. Field names are written in camelCase: `productName`, not `PRODUCT_NAME` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.productrow.add`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and make sure linking to products is enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-product-row-update-rest-v3.md)
- [{#T}](./crm-entity-product-row-replace-rest-v3.md)
- [{#T}](./crm-entity-product-row-list-rest-v3.md)
- [{#T}](./crm-entity-product-row-delete-rest-v3.md)
