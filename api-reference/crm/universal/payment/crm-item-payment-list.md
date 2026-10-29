# Get the List of Payments crm.item.payment.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires read access permission for the CRM object from which payments are selected.

The `crm.item.payment.list` method returns all payments of a deal or invoice in a single response, without pagination.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityId***
[`integer`](../../../../api-reference/data-types.md) | Identifier of the deal or invoice. You can retrieve the identifier using the [crm.item.list](../crm-item-list.md) method ||
|| **entityTypeId***
[`integer`](../../../../api-reference/data-types.md) | Identifier of the [CRM object type](../../data-types.md#object_type). Only two types have payments:
- `2` — deal
- `31` — invoice

For other types, the method returns an empty array ||
|| **filter**
[`object`](../../../../api-reference/data-types.md) | Filter by payment fields in the `{"field": "value"}` format. You can filter by the fields from the [method response](#sale_order_payment_crm_simple): `id`, `accountNumber`, `paid`, `datePaid`, `empPaidId`, `paySystemId`, `sum`, `currency`, `paySystemName`.

You can add an operator prefix to the key:
- `=` — equal to, used by default
- `!=` — not equal to
- `>`, `>=`, `<`, `<=` — greater than, greater than or equal to, less than, less than or equal to
- `@` — in an array of values

For example, `{"paid": "Y", ">sum": 1000}` returns paid payments with an amount greater than 1000. If you pass a field that the payment does not have, the method returns the `Internal server error.` error ||
|| **order**
[`object`](../../../../api-reference/data-types.md) | Sorting in the `{"field": "direction"}` format. The fields are the same as in `filter`. Direction:
- `ASC` — ascending
- `DESC` — descending

Defaults to `{"id": "ASC"}` ||
|#

The method does not support the `start` parameter: all payments of the object are returned in a single response.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13123,"entityTypeId":2,"filter":{"@id":[1036,1037]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13123,"entityTypeId":2,"filter":{"@id":[1036,1037]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each payment item returned in result[]
    type PaymentItem = {
      id: number
      accountNumber: string
      paid: 'Y' | 'N'
      datePaid: ISODate | null
      empPaidId: number | null
      paySystemId: number
      sum: number
      currency: string
      paySystemName: string
    }

    try {
      // crm.item.payment.list returns all records in one response, without pagination
      const response = await $b24.actions.v2.call.make<PaymentItem[]>({
        method: 'crm.item.payment.list',
        params: {
          entityId: 13123,
          entityTypeId: 2,
          filter: {
            '@id': [1036, 1037],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Payments:', result.length, result)
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
      async function loadPaymentList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.item.payment.list returns all records in one response, without pagination
          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.payment.list',
            params: {
              entityId: 13123,
              entityTypeId: 2,
              filter: {
                '@id': [1036, 1037],
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
          console.info('Payments:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', loadPaymentList)
    </script>
    ```

- Python

    Example

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.payment.list(
            entity_id=13123,
            entity_type_id=2,
            filter={
                "@id": [1036, 1037],
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    Example `as_list`

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.payment.list(
            entity_id=13123,
            entity_type_id=2,
            filter={
                "@id": [1036, 1037],
            },
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.payment.list',
                [
                    'entityId'     => 13123,
                    'entityTypeId' => 2,
                    'filter'       => [
                        "@id" => [1036, 1037]
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching payment list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.list', {
            entityId: 13123,
            entityTypeId: 2,
            filter: {
                "@id": [1036, 1037]
            },
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.payment.list',
        [
            'entityId' => 13123,
            'entityTypeId' => 2,
            'filter' => [
                '@id' => [1036, 1037]
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
    res, err := client.Core().Call(ctx, "crm.item.payment.list", b24.Params{
    	"entityId":     13123,
    	"entityTypeId": 2,
    	"filter": b24.Params{
    		"@id": []int{1036, 1037},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.item.payment.list: %w", err)
    }

    var items []struct {
    	ID            b24.ID `json:"id"`
    	AccountNumber string `json:"accountNumber"`
    	Paid          string `json:"paid"`
    	DatePaid      string `json:"datePaid"`
    	EmpPaidID     b24.ID `json:"empPaidId"`
    	PaySystemID   b24.ID `json:"paySystemId"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.AccountNumber)
    }

    // Total and Next are filled in by list methods; for a full
    // list traversal, use client.Core().Pages and Scan.
    if res.Total != nil {
    	fmt.Println("total:", *res.Total)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "id": 1036,
            "accountNumber": "1049/1",
            "paid": "Y",
            "datePaid": "2026-10-28T16:59:33+01:00",
            "empPaidId": 1,
            "paySystemId": 11,
            "sum": 3000,
            "currency": "USD",
            "paySystemName": "Cash"
        },
        {
            "id": 1037,
            "accountNumber": "1049/2",
            "paid": "N",
            "datePaid": null,
            "empPaidId": null,
            "paySystemId": 19,
            "sum": 3000,
            "currency": "USD",
            "paySystemName": "PayPal"
        }
    ],
    "time": {
        "start": 1793203179,
        "finish": 1793203179.129769,
        "duration": 0.12976908683776855,
        "processing": 0,
        "date_start": "2026-10-28T16:59:39+01:00",
        "date_finish": "2026-10-28T16:59:39+01:00",
        "operating_reset_at": 1793203740,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_order_payment_crm_simple[]`](#sale_order_payment_crm_simple) | Array of payments of the CRM object [(detailed description)](#sale_order_payment_crm_simple). If there are no payments, an empty array is returned ||
|| **time**
[`time`](../../../../api-reference/data-types.md) | Information about the request execution time ||
|#

#### sale_order_payment_crm_simple {#sale_order_payment_crm_simple}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Payment identifier ||
|| **accountNumber**
[`string`](../../../../api-reference/data-types.md) | Payment number in the `order number/payment sequence number` format, for example `1045/1` ||
|| **paid**
[`string`](../../../../api-reference/data-types.md) | Payment status:
- `Y` — paid
- `N` — not paid ||
|| **datePaid**
[`datetime`](../../../../api-reference/data-types.md) | Date and time when the payment was last marked as paid. `null` if the payment has never been made.

The value is kept after [crm.item.payment.unpay](./crm-item-payment-unpay.md) ||
|| **empPaidId**
[`user.id`](../../../../api-reference/data-types.md) | Identifier of the user who last marked the payment as paid. `null` if the payment has never been made.

The value is kept after [crm.item.payment.unpay](./crm-item-payment-unpay.md) ||
|| **paySystemId**
[`sale_paysystem.id`](../../../sale/data-types.md#sale_paysystem) | Payment system identifier ||
|| **sum**
[`double`](../../../../api-reference/data-types.md) | Payment amount. It is recalculated automatically when products and delivery are added to or removed from the payment ||
|| **currency**
[`string`](../../../../api-reference/data-types.md) | Payment currency ||
|| **paySystemName**
[`string`](../../../../api-reference/data-types.md) | Name of the payment system ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {entityId}"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {`parameter name`} | The required parameter `entityId` or `entityTypeId` was not provided ||
|| `400` | `100` | Internal server error. | A field that the payment does not have was passed in `filter` or `order` ||
|| `400` | `ACCESS_DENIED` | Access denied | No permission to read the CRM object ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./products-in-payment/index.md)
- [{#T}](./delivery-in-payment/index.md)
- [{#T}](./crm-item-payment-update.md)
- [{#T}](./crm-item-payment-delete.md)
- [{#T}](./crm-item-payment-get.md)
- [{#T}](./crm-item-payment-pay.md)
- [{#T}](./crm-item-payment-unpay.md)
- [{#T}](./crm-item-payment-add.md)
- [{#T}](../../../../tutorials/crm/how-to-edit-crm-objects/how-to-set-paid-date-to-deal.md)