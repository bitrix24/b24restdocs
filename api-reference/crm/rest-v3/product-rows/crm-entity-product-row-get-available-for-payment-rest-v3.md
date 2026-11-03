# Get Unpaid Product Items of an Item v 3.0 crm.[type].productrow.getavailableforpayment

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the CRM item whose product items are requested

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.productrow.getavailableforpayment` method returns the product items of a CRM item that are not yet included in payments. The object type is set by the `<type>` segment of the method name.

In the `quantity` field, the method returns the quantity remaining to be paid rather than the product item quantity. For example, a product item has 3 units, and 2 of them are already in a payment — the method returns `quantity: 1`. Product items fully included in payments are not returned. All product items of an item regardless of payments are returned by the [crm.&lt;type&gt;.productrow.list](./crm-entity-product-row-list-rest-v3.md) method.

The method returns all matching product items at once, without pagination. Payments are created with the [crm.item.payment.*](../../universal/payment/index.md) methods.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.productrow.getavailableforpayment` ||
|| Lead | `crm.lead.productrow.getavailableforpayment` ||
|| Estimate | `crm.quote.productrow.getavailableforpayment` ||
|| Invoice | `crm.smartinvoice.productrow.getavailableforpayment` ||
|| CRM document | `crm.smartdocument.productrow.getavailableforpayment` ||
|| SPA | `crm.smartprocess<entityTypeId>.productrow.getavailableforpayment`, for example, `crm.smartprocess1038.productrow.getavailableforpayment` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. An SPA has product item methods only if linking to catalog products is enabled in the type settings.

The method name is written in lowercase: `crm.deal.productrow.getavailableforpayment`, not `crm.deal.productRow.getAvailableForPayment`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerId***
[`integer`](../../../data-types.md) | CRM item ID.

The ID can be retrieved with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of fields to return for each product item, for example, `["id","productName","quantity"]`. The available fields are described in the [product item methods overview](./index.md#fields).

Default: `id` only ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.productrow.getavailableforpayment`

{% endnote %}

The examples retrieve the unpaid product items of an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerId":37,"select":["id","productId","productName","price","quantity"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.productrow.getavailableforpayment
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerId":37,"select":["id","productId","productName","price","quantity"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.productrow.getavailableforpayment
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductRowAvailableResult = {
      items: Array<{
        id: number
        productId: number
        productName: string
        price: number
        quantity: number
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ProductRowAvailableResult>(
        'crm.smartprocess1038.productrow.getavailableforpayment',
        {
          ownerId: 37,
          select: ['id', 'productId', 'productName', 'price', 'quantity'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Product rows to pay:', result.items)
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
      async function getRowsAvailableForPayment() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.productrow.getavailableforpayment',
            {
              ownerId: 37,
              select: ['id', 'productId', 'productName', 'price', 'quantity'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Product rows to pay:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getRowsAvailableForPayment)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.productrow.getavailableforpayment"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "ownerId": 37,
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
                'crm.smartprocess1038.productrow.getavailableforpayment',
                [
                    'ownerId' => 37,
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
        'crm.smartprocess1038.productrow.getavailableforpayment',
        {
            ownerId: 37,
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
        'crm.smartprocess1038.productrow.getavailableforpayment',
        [
            'ownerId' => 37,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.productrow.getavailableforpayment", b24.Params{
    	"ownerId": 37,
    	"select":  []string{"id", "productId", "productName", "price", "quantity"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.productrow.getavailableforpayment: %w", err)
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
    	fmt.Println(item.ID, item.ProductName, item.Quantity)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

Response for an item with two product items: the first one has 3 units, and 2 of them are already in a payment.

```json
{
    "result": {
        "items": [
            {
                "id": 2427,
                "productId": 6461,
                "productName": "Product",
                "price": 100.5,
                "quantity": 1
            },
            {
                "id": 2429,
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
[`object[]`](../../../data-types.md) | Unpaid product items with the fields from `select`. The `quantity` field contains the quantity remaining to be paid. If all product items are paid or there are no product items, the array is empty. [Description of product item fields](./index.md#fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 999999 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `ownerId` | Required field `ownerId` is missing | Pass the item `ownerId` ||
|| `ownerId` | This request requires data type `int` in field `ownerId` | Pass the ID as a number ||
|| `-` | This method does not support multiple orders | The item is linked to several orders, and the method cannot calculate the remaining quantity. Retrieve the product items with the [crm.&lt;type&gt;.productrow.list](./crm-entity-product-row-list-rest-v3.md) method and the item payments with the [crm.item.payment.list](../../universal/payment/crm-item-payment-list.md) method ||
|#

#### Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | This object type has no item with the `ownerId` ID, or you do not have permission to view it. Check `ownerId` and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductRowDto` | Check the field names in `select` against the [description of product item fields](./index.md#fields) ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.productrow.getavailableforpayment`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and make sure linking to products is enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-product-row-list-rest-v3.md)
- [{#T}](../../universal/payment/index.md)
