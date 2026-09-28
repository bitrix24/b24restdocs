# Add Order sale.order.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

Creates an order without basket items, payments, or shipments and returns its fields. Add basket items, payments, and shipments after the order is created, using the [sale.basketitem.*](../basket-item/index.md), [sale.payment.*](../payment/index.md), and [sale.shipment.*](../shipment/index.md) methods.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating an order [(detailed description)](#params-fields) ||
|#

### Parameter fields {#params-fields}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`string`](../../data-types.md) | Identifier of the site the order belongs to. In cloud Bitrix24, pass `s1`. The field cannot be changed after the order is created ||
|| **personTypeId***
[`sale_person_type.id`](../data-types.md#sale_person_type) | Identifier of the payer type, such as an individual or a legal entity. Retrieve it using the [sale.persontype.list](../person-type/sale-person-type-list.md) method. The method does not check that the payer type exists. The field cannot be changed after the order is created ||
|| **currency***
[`string`](../../data-types.md) | Currency code, such as `USD`. Retrieve the list of currencies using the [crm.currency.list](../../crm/currency/crm-currency-list.md) method. The field cannot be changed after the order is created ||
|| **price**
[`double`](../../data-types.md) | Order amount including delivery ||
|| **discountValue**
[`double`](../../data-types.md) | Discount value ||
|| **statusId**
[`sale_status.id`](../data-types.md#sale_status) | Identifier of the order status. Retrieve the list of statuses using the [sale.status.list](../status/sale-status-list.md) method. If the field is not provided, the order receives the initial status `N` ||
|| **empStatusId**
[`user.id`](../../data-types.md) | Identifier of the user who changed the order status ||
|| **dateInsert**
[`datetime`](../../data-types.md) | Order creation date ||
|| **marked**
[`string`](../../data-types.md) | Indicates whether the order is marked as problematic. Bitrix24 sets `Y` automatically if a warning occurs while the order is being saved and writes the reason to the `reasonMarked` field.

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **empMarkedId**
[`user.id`](../../data-types.md) | Identifier of the user who set the marking ||
|| **reasonMarked**
[`string`](../../data-types.md) | Reason the order is marked as problematic ||
|| **userDescription**
[`string`](../../data-types.md) | Customer's comment on the order ||
|| **additionalInfo**
[`string`](../../data-types.md) | Deprecated.

Additional information ||
|| **comments**
[`string`](../../data-types.md) | Manager's comment on the order ||
|| **companyId**
[`integer`](../../data-types.md) | Identifier of the company from the Online Store module ||
|| **responsibleId**
[`user.id`](../../data-types.md) | Identifier of the user responsible for the order ||
|| **recurringId**
[`string`](../../data-types.md) | Identifier for subscription renewal ||
|| **lockedBy**
[`string`](../../data-types.md) | Relevant only for on-premise.

Identifier of the user who locked the order. The order is locked in the admin panel when the user opens the detail form of the order ||
|| **recountFlag**
[`string`](../../data-types.md) | Deprecated.

Recount flag.

- `Y` — yes
- `N` — no

Defaults to `Y` ||
|| **affiliateId**
[`integer`](../../data-types.md) | Relevant only for on-premise.

Identifier of the affiliate ||
|| **updated1c**
[`string`](../../data-types.md) | Whether the order was updated via an ERP system.

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **orderTopic**
[`string`](../../data-types.md) | Deprecated.

Order topic ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier ||
|| **id1c**
[`string`](../../data-types.md) | Identifier in the ERP system ||
|| **version1c**
[`string`](../../data-types.md) | Version in the ERP system ||
|| **externalOrder**
[`string`](../../data-types.md) | Whether the order is from an external system.

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **canceled**
[`string`](../../data-types.md) | Whether the order was canceled.

- `Y` — yes
- `N` — no

Defaults to `N` ||
|| **empCanceledId**
[`user.id`](../../data-types.md) | Identifier of the user who canceled the order ||
|| **reasonCanceled**
[`string`](../../data-types.md) | Reason for cancellation ||
|| **userId**
[`user.id`](../../data-types.md) | Identifier of the customer. The field cannot be changed after the order is created ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"lid":"s1","personTypeId":1,"currency":"USD","statusId":"N","userId":1,"responsibleId":1,"userDescription":"Call before delivery","comments":"Order from the website integration"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.order.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"lid":"s1","personTypeId":1,"currency":"USD","statusId":"N","userId":1,"responsibleId":1,"userDescription":"Call before delivery","comments":"Order from the website integration"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.order.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type OrderResult = {
      order: {
        accountNumber: string
        canceled: string
        clients: Record<string, unknown>[]
        comments: string
        currency: string
        dateInsert: ISODate | null
        dateStatus: ISODate | null
        dateUpdate: ISODate | null
        deducted: string
        empStatusId: number
        id: number
        lid: string
        payed: string
        personTypeId: number
        personTypeXmlId: string
        propertyValues: Record<string, unknown>[]
        requisiteLink: Record<string, number>
        responsibleId: number
        statusId: string
        statusXmlId: string
        updated1c: string
        userDescription: string
        userId: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<OrderResult>({
        method: 'sale.order.add',
        params: {
          fields: {
            lid: 's1',
            personTypeId: 1,
            currency: 'USD',
            statusId: 'N',
            userId: 1,
            responsibleId: 1,
            userDescription: 'Call before delivery',
            comments: 'Order from the website integration',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.order.id, result.order.accountNumber)
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
      async function addOrder() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.order.add',
            params: {
              fields: {
                lid: 's1',
                personTypeId: 1,
                currency: 'USD',
                statusId: 'N',
                userId: 1,
                responsibleId: 1,
                userDescription: 'Call before delivery',
                comments: 'Order from the website integration',
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
          console.info(result.order.id, result.order.accountNumber)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addOrder)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "lid": "s1",
        "personTypeId": 1,
        "currency": "USD",
        "statusId": "N",
        "userId": 1,
        "responsibleId": 1,
        "userDescription": "Call before delivery",
        "comments": "Order from the website integration",
    }

    try:
        bitrix_response = client.sale.order.add(
            fields=fields,
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
                'sale.order.add',
                [
                    'fields' => [
                        'lid'             => 's1',
                        'personTypeId'    => 1,
                        'currency'        => 'USD',
                        'statusId'        => 'N',
                        'userId'          => 1,
                        'responsibleId'   => 1,
                        'userDescription' => 'Call before delivery',
                        'comments'        => 'Order from the website integration',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding order: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.order.add',
        {
            fields: {
                lid: 's1',
                personTypeId: 1,
                currency: 'USD',
                statusId: 'N',
                userId: 1,
                responsibleId: 1,
                userDescription: 'Call before delivery',
                comments: 'Order from the website integration',
            }
        },
        function(result)
        {
            if(result.error())
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
        'sale.order.add',
        [
            'fields' => [
                'lid' => 's1',
                'personTypeId' => 1,
                'currency' => 'USD',
                'statusId' => 'N',
                'userId' => 1,
                'responsibleId' => 1,
                'userDescription' => 'Call before delivery',
                'comments' => 'Order from the website integration',
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
    res, err := client.Core().Call(ctx, "sale.order.add", b24.Params{
    	"fields": b24.Params{
    		"lid":             "s1",
    		"personTypeId":    1,
    		"currency":        "EUR",
    		"statusId":        "N",
    		"userId":          1,
    		"responsibleId":   1,
    		"userDescription": "Call before delivery",
    		"comments":        "Order from the website integration",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.order.add: %w", err)
    }

    // The method wraps the response in an object with the "order" key.
    raw, ok := b24.Unwrap(res.Result, "order")
    if !ok {
    	return fmt.Errorf("no order key in the response")
    }

    var item struct {
    	ID            b24.ID `json:"id"`
    	AccountNumber string `json:"accountNumber"`
    	StatusID      string `json:"statusId"`
    	Comments      string `json:"comments"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.AccountNumber, item.StatusID, item.Comments)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "order": {
            "accountNumber": "971",
            "canceled": "N",
            "clients": [
                {
                    "entityId": 2819,
                    "entityTypeId": 3,
                    "id": 1717,
                    "isPrimary": "Y",
                    "orderId": 971
                }
            ],
            "comments": "Order from the website integration",
            "currency": "USD",
            "dateInsert": "2026-09-28T08:02:16+02:00",
            "dateStatus": "2026-09-28T08:02:15+02:00",
            "dateUpdate": "2026-09-28T08:02:16+02:00",
            "deducted": "N",
            "empStatusId": 1,
            "id": 971,
            "lid": "s1",
            "payed": "N",
            "personTypeId": 1,
            "personTypeXmlId": "",
            "propertyValues": [
                {
                    "code": "EMAIL",
                    "id": 11287,
                    "name": "E-Mail",
                    "orderPropsId": 41,
                    "orderPropsXmlId": "bx_60b605ba1d082"
                },
                {
                    "code": "FIO",
                    "id": 11289,
                    "name": "Full name",
                    "orderPropsId": 39,
                    "orderPropsXmlId": "bx_609bec7cc794c"
                }
            ],
            "requisiteLink": {
                "mcBankDetailId": 0,
                "mcRequisiteId": 0,
                "requisiteId": 467
            },
            "responsibleId": 1,
            "statusId": "N",
            "statusXmlId": "",
            "updated1c": "N",
            "userDescription": "Call before delivery",
            "userId": 1,
            "xmlId": "bx_6aba02e7a86af"
        }
    },
    "time": {
        "start": 1790575335,
        "finish": 1790575336.454058,
        "duration": 1.4540579319000244,
        "processing": 1,
        "date_start": "2026-09-28T09:02:15+02:00",
        "date_finish": "2026-09-28T09:02:16+02:00",
        "operating_reset_at": 1790575935,
        "operating": 0.7697091102600098
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`sale_order`](../data-types.md#sale_order) | The created order. The order identifier to use in other methods is in the `id` field. The response includes only the populated order fields, plus `clients`, `requisiteLink`, and `propertyValues` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: personTypeId, currency, lid"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | `Required fields: personTypeId, currency, lid` | Required fields are not provided; their names are listed in the error text ||
|| `400` | `100` | `Could not find value for parameter {fields}` | The `fields` parameter is not provided ||
|| `400` | `200040300020` | `Access Denied` | Insufficient permissions to add an order ||
|| `400` | `0` | Save error text | The order was not saved for another reason, which is given in `error_description` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-order-update.md)
- [{#T}](./sale-order-get.md)
- [{#T}](./sale-order-list.md)
- [{#T}](./sale-order-delete.md)
- [{#T}](./sale-order-get-fields.md)
