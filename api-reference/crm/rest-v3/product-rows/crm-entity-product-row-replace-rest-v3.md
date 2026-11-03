# Replace All Product Items of an Item v 3.0 crm.[type].productrow.replace

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM item whose product items are replaced

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.productrow.replace` method replaces the entire set of product items of a CRM item with the passed set and returns the product items the item ends up with. The object type is set by the `<type>` segment of the method name. Product items that are not in the set are deleted. An empty set deletes all product items of the item.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.productrow.replace` ||
|| Lead | `crm.lead.productrow.replace` ||
|| Estimate | `crm.quote.productrow.replace` ||
|| Invoice | `crm.smartinvoice.productrow.replace` ||
|| CRM document | `crm.smartdocument.productrow.replace` ||
|| SPA | `crm.smartprocess<entityTypeId>.productrow.replace`, for example, `crm.smartprocess1038.productrow.replace` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. An SPA has product item methods only if linking to catalog products is enabled in the type settings.

The method name is written in lowercase: `crm.smartinvoice.productrow.replace`, not `crm.smartInvoice.productRow.replace`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerId***
[`integer`](../../../data-types.md) | ID of the CRM item whose product items are replaced.

The ID can be retrieved with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| **items***
[`object[]`](../../../data-types.md) | New set of product items. Each product item is an object with fields in the format `{"field": "value"}` and must contain at least one field. [Parameter description](#items)

An empty array `[]` deletes all product items of the item. The set contains no more than 1,000 product items ||
|| **select**
[`array`](../../../data-types.md) | List of fields to return for each product item in the response, for example, `["id","productName","price"]`. The available fields are described in the [product item methods overview](./index.md#fields).

Default: `id` only ||
|#

### The items Parameter {#items}

A product item of the set takes the same fields as the [fields parameter of the crm.&lt;type&gt;.productrow.add method](./crm-entity-product-row-add-rest-v3.md#fields), except `ownerId`: the item is set by the `ownerId` parameter of the method. You can pass `productId`, `productName`, `price`, `quantity`, `discountTypeId`, `discountRate`, `discountSum`, `taxRate`, `taxIncluded`, `measureCode`, `measureName`, and `sort`.

The `id` field cannot be passed in a product item: the method does not update existing product items by ID but compares the new set with the current one. A product item that matches the current one in all fields keeps its ID. A changed product item is written anew and gets a new `id`. The current IDs are returned in the response.

To change one product item and keep its ID, use the [crm.&lt;type&gt;.productrow.update](./crm-entity-product-row-update-rest-v3.md) method.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.productrow.replace`

{% endnote %}

The examples leave two product items in an SPA item with the type ID `1038`: a catalog product and a product item without a catalog product. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerId":37,"items":[{"productId":6461,"price":100.5,"quantity":1},{"productName":"Delivery","price":700,"quantity":1}],"select":["id","productId","productName","price","quantity"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.productrow.replace
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerId":37,"items":[{"productId":6461,"price":100.5,"quantity":1},{"productName":"Delivery","price":700,"quantity":1}],"select":["id","productId","productName","price","quantity"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.productrow.replace
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductRowReplaceResult = {
      items: Array<{
        id: number
        productId: number
        productName: string
        price: number
        quantity: number
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ProductRowReplaceResult>(
        'crm.smartprocess1038.productrow.replace',
        {
          ownerId: 37,
          items: [
            {
              productId: 6461,
              price: 100.5,
              quantity: 1,
            },
            {
              productName: 'Delivery',
              price: 700,
              quantity: 1,
            },
          ],
          select: ['id', 'productId', 'productName', 'price', 'quantity'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Product rows after replace:', result.items)
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
      async function replaceProductRows() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.productrow.replace',
            {
              ownerId: 37,
              items: [
                {
                  productId: 6461,
                  price: 100.5,
                  quantity: 1,
                },
                {
                  productName: 'Delivery',
                  price: 700,
                  quantity: 1,
                },
              ],
              select: ['id', 'productId', 'productName', 'price', 'quantity'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Product rows after replace:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', replaceProductRows)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.productrow.replace"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "ownerId": 37,
                "items": [
                    {
                        "productId": 6461,
                        "price": 100.5,
                        "quantity": 1,
                    },
                    {
                        "productName": "Delivery",
                        "price": 700,
                        "quantity": 1,
                    },
                ],
                "select": ["id", "productId", "productName", "price", "quantity"],
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
                'crm.smartprocess1038.productrow.replace',
                [
                    'ownerId' => 37,
                    'items' => [
                        [
                            'productId' => 6461,
                            'price' => 100.5,
                            'quantity' => 1,
                        ],
                        [
                            'productName' => 'Delivery',
                            'price' => 700,
                            'quantity' => 1,
                        ],
                    ],
                    'select' => ['id', 'productId', 'productName', 'price', 'quantity'],
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
        'crm.smartprocess1038.productrow.replace',
        {
            ownerId: 37,
            items: [
                {
                    productId: 6461,
                    price: 100.5,
                    quantity: 1,
                },
                {
                    productName: 'Delivery',
                    price: 700,
                    quantity: 1,
                },
            ],
            select: ['id', 'productId', 'productName', 'price', 'quantity'],
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
        'crm.smartprocess1038.productrow.replace',
        [
            'ownerId' => 37,
            'items' => [
                [
                    'productId' => 6461,
                    'price' => 100.5,
                    'quantity' => 1,
                ],
                [
                    'productName' => 'Delivery',
                    'price' => 700,
                    'quantity' => 1,
                ],
            ],
            'select' => ['id', 'productId', 'productName', 'price', 'quantity'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.productrow.replace", b24.Params{
    	"ownerId": 37,
    	"items": []b24.Params{
    		{
    			"productId": 6461,
    			"price":     100.5,
    			"quantity":  1,
    		},
    		{
    			"productName": "Delivery",
    			"price":       700,
    			"quantity":    1,
    		},
    	},
    	"select": []string{"id", "productId", "productName", "price", "quantity"},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.productrow.replace: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID          int     `json:"id"`
    		ProductID   int     `json:"productId"`
    		ProductName string  `json:"productName"`
    		Price       float64 `json:"price"`
    		Quantity    float64 `json:"quantity"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.ProductName, item.Price, item.Quantity)
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
                "id": 2413,
                "productId": 6461,
                "productName": "Product",
                "price": 100.5,
                "quantity": 1
            },
            {
                "id": 2415,
                "productId": 0,
                "productName": "Delivery",
                "price": 700,
                "quantity": 1
            }
        ]
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
|| **result.items**
[`object[]`](../../../data-types.md) | Product items of the item after the replacement, with the fields from `select`. Without `select`, each product item contains only `id`. After a replacement with an empty set, the array is empty. [Description of product item fields](./index.md#fields) ||
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
                "message": "Field \"id\" is read-only.",
                "field": "items.0.id"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

An error in a product item of the set specifies its index in `items`, starting from `0`: `items.0.id` is the `id` field of the first product item. If the set contains an error, the method does not change the product items of the item.

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `ownerId` | Required field `ownerId` is missing | Pass the item `ownerId` ||
|| `items` | Required field `items` is missing | Pass the `items` array. To delete all product items, pass `[]` ||
|| `items` | The list `items` holds more than 1000 elements | Pass no more than 1,000 product items ||
|| `items.0` | This request requires data type `ProductRowDto` in field `items.0` | Pass each product item as an object ||
|| `items.0` | Product row 0 has no editable fields | Pass at least one field in the product item ||
|| `items.0.ownerId` | The product row field `ownerId` is read-only | Remove `ownerId` from the product items of the set: the item is set by the `ownerId` parameter of the method ||
|| `price` | This request requires data type `float` in field `price` | Pass a value of the type specified in the field description. This error does not include the product item index, so find the product item by the field name ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `items.0.id` | Field `id` is read-only | Remove `id` from the product items of the set. The method responds the same way to `ownerTypeId` and `productTypeId` ||
|| `items.0.productId` | Field value is not one of the possible values | The product is not in the catalog, or you do not have access to it. Check `productId` with the [catalog.product.list](../../../catalog/product/catalog-product-list-rest-v3.md) method ||
|#

#### Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | This object type has no item with the `ownerId` ID, or you do not have permission to edit it. Check `ownerId` and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `items.0.foo` found in entity `ProductRowDto` | Check the field name in the product item with the specified index. Field names are written in camelCase: `productName`, not `PRODUCT_NAME` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.productrow.replace`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and make sure linking to products is enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-product-row-add-rest-v3.md)
- [{#T}](./crm-entity-product-row-update-rest-v3.md)
- [{#T}](./crm-entity-product-row-list-rest-v3.md)
