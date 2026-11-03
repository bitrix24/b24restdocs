# Update a Product Item v 3.0 crm.[type].productrow.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM item to which the product item belongs

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.productrow.update` method updates the fields of a product item and returns the product item after the write. The object type is set by the `<type>` segment of the method name. The method changes only the passed fields, and the product item ID stays the same.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.productrow.update` ||
|| Lead | `crm.lead.productrow.update` ||
|| Estimate | `crm.quote.productrow.update` ||
|| Invoice | `crm.smartinvoice.productrow.update` ||
|| CRM document | `crm.smartdocument.productrow.update` ||
|| SPA | `crm.smartprocess<entityTypeId>.productrow.update`, for example, `crm.smartprocess1038.productrow.update` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. An SPA has product item methods only if linking to catalog products is enabled in the type settings.

The method name is written in lowercase: `crm.smartinvoice.productrow.update`, not `crm.smartInvoice.productRow.update`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Product item ID.

The ID can be retrieved with the [crm.&lt;type&gt;.productrow.list](./crm-entity-product-row-list-rest-v3.md) method. The product item must belong to an item of the type set in the method name ||
|| **fields***
[`object`](../../../data-types.md) | Object with the new field values in the format `{"field": "value"}`. Pass at least one field.

You can change the `productId`, `productName`, `price`, `quantity`, `discountTypeId`, `discountRate`, `discountSum`, `taxRate`, `taxIncluded`, `measureCode`, `measureName`, and `sort` fields. They are described in the [fields parameter table of the crm.&lt;type&gt;.productrow.add method](./crm-entity-product-row-add-rest-v3.md#fields).

The `ownerId` field cannot be changed: a product item cannot be moved to another item. To move a product item, delete it with the [crm.&lt;type&gt;.productrow.delete](./crm-entity-product-row-delete-rest-v3.md) method and add it to the target item with the [crm.&lt;type&gt;.productrow.add](./crm-entity-product-row-add-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of product item fields to return in the response, for example, `["id","price","quantity"]`. The available fields are described in the [product item methods overview](./index.md#fields).

Default: `id` only ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.productrow.update`

{% endnote %}

The examples change the price and quantity of a product item in an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2411,"fields":{"price":4500,"quantity":3},"select":["id","productName","price","quantity","sort"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.productrow.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2411,"fields":{"price":4500,"quantity":3},"select":["id","productName","price","quantity","sort"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.productrow.update
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductRowUpdateResult = {
      item: {
        id: number
        productName: string
        price: number
        quantity: number
        sort: number
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ProductRowUpdateResult>(
        'crm.smartprocess1038.productrow.update',
        {
          id: 2411,
          fields: {
            price: 4500,
            quantity: 3,
          },
          select: ['id', 'productName', 'price', 'quantity', 'sort'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Product row updated:', result.item)
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
      async function updateProductRow() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.productrow.update',
            {
              id: 2411,
              fields: {
                price: 4500,
                quantity: 3,
              },
              select: ['id', 'productName', 'price', 'quantity', 'sort'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Product row updated:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateProductRow)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.productrow.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 2411,
                "fields": {
                    "price": 4500,
                    "quantity": 3,
                },
                "select": ["id", "productName", "price", "quantity", "sort"],
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
                'crm.smartprocess1038.productrow.update',
                [
                    'id' => 2411,
                    'fields' => [
                        'price' => 4500,
                        'quantity' => 3,
                    ],
                    'select' => ['id', 'productName', 'price', 'quantity', 'sort'],
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
        'crm.smartprocess1038.productrow.update',
        {
            id: 2411,
            fields: {
                price: 4500,
                quantity: 3,
            },
            select: ['id', 'productName', 'price', 'quantity', 'sort'],
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
        'crm.smartprocess1038.productrow.update',
        [
            'id' => 2411,
            'fields' => [
                'price' => 4500,
                'quantity' => 3,
            ],
            'select' => ['id', 'productName', 'price', 'quantity', 'sort'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.productrow.update", b24.Params{
    	"id": 2411,
    	"fields": b24.Params{
    		"price":    4500,
    		"quantity": 3,
    	},
    	"select": []string{"id", "productName", "price", "quantity", "sort"},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.productrow.update: %w", err)
    }

    var result struct {
    	Item struct {
    		ID          int     `json:"id"`
    		ProductName string  `json:"productName"`
    		Price       float64 `json:"price"`
    		Quantity    float64 `json:"quantity"`
    		Sort        int     `json:"sort"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.Price, result.Item.Quantity)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 2411,
            "productName": "Installation",
            "price": 4500,
            "quantity": 3,
            "sort": 0
        }
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
|| **result.item**
[`object`](../../../data-types.md) | Product item after the update, with the fields from `select`. Without `select`, the object contains only `id`. [Description of product item fields](./index.md#fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "The product row field \"ownerId\" is read-only.",
                "field": "ownerId"
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
|| `id` | Required field `id` is missing | Pass the product item `id` ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `fields` | No product row fields specified | Pass at least one field in `fields` ||
|| `ownerId` | The product row field `ownerId` is read-only | Remove `ownerId` from `fields`. To move a product item to another item, delete it and add it again ||
|| `price` | This request requires data type `float` in field `price` | Pass a value of the type specified in the field description ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `productId` | Field value is not one of the possible values | The product is not in the catalog, or you do not have access to it. Check `productId` with the [catalog.product.list](../../../catalog/product/catalog-product-list-rest-v3.md) method ||
|| `id` | Field `id` is read-only | Remove the read-only field from `fields`. The method responds the same way to `ownerTypeId` and `productTypeId` ||
|#

#### Product Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | The product item does not exist, belongs to an item of a different object type, or you do not have permission to edit the item. Check `id` with the [crm.&lt;type&gt;.productrow.list](./crm-entity-product-row-list-rest-v3.md) method and check the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductRowDto` | Check the field name in `fields` or `select`. Field names are written in camelCase: `productName`, not `PRODUCT_NAME` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.productrow.update`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and make sure linking to products is enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-product-row-add-rest-v3.md)
- [{#T}](./crm-entity-product-row-get-rest-v3.md)
- [{#T}](./crm-entity-product-row-replace-rest-v3.md)
- [{#T}](./crm-entity-product-row-delete-rest-v3.md)
