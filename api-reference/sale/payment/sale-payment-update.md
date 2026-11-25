# Update Payment sale.payment.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.payment.update` updates an existing order payment.

Pass only the fields to change in `fields`, plus the required `paySystemId`: it is required on every update, even if the payment system does not change. Fields you do not pass retain their values, but changing the payment status, return, or responsible person can automatically update related service fields.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_payment.id`](../data-types.md#sale_order_payment) | Payment identifier ||
|| **fields***
[`object`](../../data-types.md) | Fields for updating a payment [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paySystemId***
[`sale_paysystem.id`](../data-types.md#sale_paysystem) | Payment system identifier. Required on every update ||
|| **paid**
[`string`](../../data-types.md) | Paid flag:

- `Y` — yes
- `N` — no ||
|| **datePaid**
[`datetime`](../../data-types.md) | Payment date ||
|| **empPaidId**
[`user.id`](../../data-types.md) | Identifier of the user who made the payment. When the payment switches to `paid = Y`, the field is set to the current user ||
|| **psStatus**
[`string`](../../data-types.md) | Whether the payment succeeded according to the payment system. Options:

- `Y` — yes
- `N` — no ||
|| **psStatusCode**
[`string`](../../data-types.md) | Payment system status code ||
|| **psStatusDescription**
[`string`](../../data-types.md) | Description of the payment system's operation result ||
|| **psStatusMessage**
[`string`](../../data-types.md) | Message from the payment system ||
|| **psSum**
[`double`](../../data-types.md) | Payment system amount ||
|| **psCurrency**
[`string`](../../data-types.md) | Payment system currency ||
|| **psResponseDate**
[`datetime`](../../data-types.md) | Payment system response date ||
|| **payVoucherNum**
[`string`](../../data-types.md) | Payment document number ||
|| **payVoucherDate**
[`datetime`](../../data-types.md) | Payment document date ||
|| **datePayBefore**
[`datetime`](../../data-types.md) | Invoice payment due date ||
|| **dateBill**
[`datetime`](../../data-types.md) | Invoice date ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier ||
|| **sum**
[`double`](../../data-types.md) | Payment amount. The amount of a payment that is already paid cannot be changed ||
|| **companyId**
[`integer`](../../data-types.md) | Identifier of the company that will receive the payment ||
|| **payReturnNum**
[`string`](../../data-types.md) | Return document number ||
|| **priceCod**
[`double`](../../data-types.md) | Cost of payment upon delivery. Used for cash on delivery ||
|| **payReturnDate**
[`date`](../../data-types.md) | Return document date ||
|| **empReturnId**
[`user.id`](../../data-types.md) | Identifier of the user who processed the return ||
|| **payReturnComment**
[`string`](../../data-types.md) | Comment on the return ||
|| **responsibleId**
[`user.id`](../../data-types.md) | Identifier of the user responsible for the payment ||
|| **empResponsibleId**
[`user.id`](../../data-types.md) | Identifier of the user who assigned the responsible person. When `responsibleId` changes, the field is set to the current user ||
|| **isReturn**
[`string`](../../data-types.md) | Return mode:

- `Y` — to the internal account
- `P` — through the payment system
- `N` — no return ||
|| **comments**
[`string`](../../data-types.md) | Comments on the payment ||
|| **updated1c**
[`string`](../../data-types.md) | Whether the payment was updated through an ERP system:

- `Y` — yes
- `N` — no ||
|| **id1c**
[`string`](../../data-types.md) | Identifier in the ERP system ||
|| **version1c**
[`string`](../../data-types.md) | Version of the payment document from the ERP system ||
|| **externalPayment**
[`string`](../../data-types.md) | Whether it is an external payment. Used for import from an ERP system via XML

- `Y` — yes
- `F` — yes, loaded with the order
- `N` — no ||
|| **psInvoiceId**
[`string`](../../data-types.md) | Payment identifier in the payment system ||
|| **marked**
[`string`](../../data-types.md) | Indicates whether the payment is marked as problematic:

- `Y` — yes
- `N` — no ||
|| **reasonMarked**
[`string`](../../data-types.md) | Reason for marking ||
|| **empMarkedId**
[`user.id`](../../data-types.md) | Identifier of the user who marked the payment ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":144,"fields":{"paySystemId":1,"paid":"Y","datePaid":"2024-04-10T10:00:00","empPaidId":1,"psStatus":"Y","psSum":100,"psCurrency":"USD","psResponseDate":"2024-04-10T10:00:00","sum":100,"companyId":1,"responsibleId":1,"empResponsibleId":1,"isReturn":"N","externalPayment":"N","psInvoiceId":"1","marked":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.payment.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":144,"fields":{"paySystemId":1,"paid":"Y","datePaid":"2024-04-10T10:00:00","empPaidId":1,"psStatus":"Y","psSum":100,"psCurrency":"USD","psResponseDate":"2024-04-10T10:00:00","sum":100,"companyId":1,"responsibleId":1,"empResponsibleId":1,"isReturn":"N","externalPayment":"N","psInvoiceId":"1","marked":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.payment.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PaymentUpdateResult = {
      payment: {
        accountNumber: string,
        comments: string,
        companyId: number | null,
        currency: string,
        dateBill: ISODate | null,
        dateMarked: ISODate | null,
        datePaid: ISODate | null,
        datePayBefore: ISODate | null,
        dateResponsibleId: ISODate | null,
        empMarkedId: number | null,
        empPaidId: number | null,
        empResponsibleId: number | null,
        empReturnId: number | null,
        externalPayment: string,
        id: number,
        id1c: string,
        isReturn: string,
        marked: string,
        orderId: number,
        paid: string,
        payReturnComment: string,
        payReturnDate: ISODate | null,
        payReturnNum: string,
        paySystemId: number,
        paySystemIsCash: string,
        paySystemName: string,
        paySystemXmlId: string,
        payVoucherDate: ISODate | null,
        payVoucherNum: string,
        priceCod: number,
        psCurrency: string,
        psInvoiceId: string,
        psResponseDate: ISODate | null,
        psStatus: string,
        psStatusCode: string,
        psStatusDescription: string,
        psStatusMessage: string,
        psSum: number | null,
        reasonMarked: string,
        responsibleId: number | null,
        sum: number,
        updated1c: string,
        version1c: string,
        xmlId: string,
      },
    }

    try {
      const response = await $b24.actions.v2.call.make<PaymentUpdateResult>({
        method: 'sale.payment.update',
        params: {
          id: 144,
          fields: {
            paySystemId: 1,
            paid: 'Y',
            datePaid: '2024-04-10T10:00:00',
            empPaidId: 1,
            psStatus: 'Y',
            psSum: 100,
            psCurrency: 'USD',
            psResponseDate: '2024-04-10T10:00:00',
            sum: 100,
            companyId: 1,
            responsibleId: 1,
            empResponsibleId: 1,
            isReturn: 'N',
            externalPayment: 'N',
            psInvoiceId: '1',
            marked: 'N',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated payment:', result.payment.id, 'paid:', result.payment.paid)
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
      async function updatePayment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.payment.update',
            params: {
              id: 144,
              fields: {
                paySystemId: 1,
                paid: 'Y',
                datePaid: '2024-04-10T10:00:00',
                empPaidId: 1,
                psStatus: 'Y',
                psSum: 100,
                psCurrency: 'USD',
                psResponseDate: '2024-04-10T10:00:00',
                sum: 100,
                companyId: 1,
                responsibleId: 1,
                empResponsibleId: 1,
                isReturn: 'N',
                externalPayment: 'N',
                psInvoiceId: '1',
                marked: 'N',
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
          console.info('Updated payment:', result.payment.id, 'paid:', result.payment.paid)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updatePayment)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "paySystemId": 1,
        "paid": "Y",
        "datePaid": "2024-04-10T10:00:00",
        "empPaidId": 1,
        "psStatus": "Y",
        "psSum": 100,
        "psCurrency": "USD",
        "psResponseDate": "2024-04-10T10:00:00",
        "sum": 100,
        "companyId": 1,
        "responsibleId": 1,
        "empResponsibleId": 1,
        "isReturn": "N",
        "externalPayment": "N",
        "psInvoiceId": "1",
        "marked": "N",
    }

    try:
        bitrix_response = client.sale.payment.update(
            bitrix_id=144,
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
                'sale.payment.update',
                [
                    'id' => 144,
                    'fields' => [
                        'paySystemId'          => 1,
                        'paid'                 => 'Y',
                        'datePaid'             => '2024-04-10T10:00:00',
                        'empPaidId'            => 1,
                        'psStatus'             => 'Y',
                        'psSum'                => 100,
                        'psCurrency'           => 'USD',
                        'psResponseDate'       => '2024-04-10T10:00:00',
                        'sum'                  => 100,
                        'companyId'            => 1,
                        'responsibleId'        => 1,
                        'empResponsibleId'     => 1,
                        'isReturn'             => 'N',
                        'externalPayment'      => 'N',
                        'psInvoiceId'          => '1',
                        'marked'               => 'N',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating payment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.payment.update',
        {
            id: 144,
            fields: {
                paySystemId: 1,
                paid: 'Y',
                datePaid: '2024-04-10T10:00:00',
                empPaidId: 1,
                psStatus: 'Y',
                psSum: 100,
                psCurrency: 'USD',
                psResponseDate: '2024-04-10T10:00:00',
                sum: 100,
                companyId: 1,
                responsibleId: 1,
                empResponsibleId: 1,
                isReturn: 'N',
                externalPayment: 'N',
                psInvoiceId: '1',
                marked: 'N',
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.payment.update',
        [
            'id' => 144,
            'fields' => [
                'paySystemId' => 1,
                'paid' => 'Y',
                'datePaid' => '2024-04-10T10:00:00',
                'empPaidId' => 1,
                'psStatus' => 'Y',
                'psSum' => 100,
                'psCurrency' => 'USD',
                'psResponseDate' => '2024-04-10T10:00:00',
                'sum' => 100,
                'companyId' => 1,
                'responsibleId' => 1,
                'empResponsibleId' => 1,
                'isReturn' => 'N',
                'externalPayment' => 'N',
                'psInvoiceId' => '1',
                'marked' => 'N',
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
    res, err := client.Core().Call(ctx, "sale.payment.update", b24.Params{
    	"id": 144,
    	"fields": b24.Params{
    		"paySystemId":      1,
    		"paid":             "Y",
    		"datePaid":         "2024-04-10T10:00:00",
    		"empPaidId":        1,
    		"psStatus":         "Y",
    		"psSum":            100,
    		"psCurrency":       "EUR",
    		"psResponseDate":   "2024-04-10T10:00:00",
    		"sum":              100,
    		"companyId":        1,
    		"responsibleId":    1,
    		"empResponsibleId": 1,
    		"isReturn":         "N",
    		"externalPayment":  "N",
            "psInvoiceId":      "1",
    		"marked":           "N",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.payment.update: %w", err)
    }

    // The method wraps the response in an object with the "payment" key.
    raw, ok := b24.Unwrap(res.Result, "payment")
    if !ok {
    	return fmt.Errorf("no payment key in the response")
    }

    var item struct {
    	AccountNumber string `json:"accountNumber"`
    	Comments      string `json:"comments"`
    	CompanyID     b24.ID `json:"companyId"`
    	Currency      string `json:"currency"`
    	DateBill      string `json:"dateBill"`
    	DateMarked    string `json:"dateMarked"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.AccountNumber, item.Comments)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "payment": {
            "accountNumber": "356\/1",
            "comments": "",
            "companyId": 1,
            "currency": "USD",
            "dateBill": "2024-04-10T10:00:00+02:00",
            "dateMarked": "2024-04-16T16:32:49+02:00",
            "datePaid": "2024-04-10T10:00:00+02:00",
            "datePayBefore": "2024-04-10T10:00:00+02:00",
            "dateResponsibleId": "2024-04-16T16:32:49+02:00",
            "empMarkedId": 1,
            "empPaidId": 1,
            "empResponsibleId": 1,
            "empReturnId": 1,
            "externalPayment": "N",
            "id": 144,
            "id1c": "",
            "isReturn": "N",
            "marked": "N",
            "orderId": 200,
            "paid": "Y",
            "payReturnComment": "",
            "payReturnDate": "2024-04-10T02:00:00+02:00",
            "payReturnNum": "",
            "paySystemId": 1,
            "paySystemIsCash": "N",
            "paySystemName": "Bank Transfer (Company)",
            "paySystemXmlId": "",
            "payVoucherDate": "2024-04-10T10:00:00+02:00",
            "payVoucherNum": "",
            "priceCod": 100,
            "psCurrency": "USD",
            "psInvoiceId": "1",
            "psResponseDate": "2024-04-10T10:00:00+02:00",
            "psStatus": "Y",
            "psStatusCode": "",
            "psStatusDescription": "",
            "psStatusMessage": "",
            "psSum": 100,
            "reasonMarked": "",
            "responsibleId": 1,
            "sum": 100,
            "updated1c": "N",
            "version1c": "",
            "xmlId": ""
        }
    },
    "time": {
        "start": 1713454774.293692,
        "finish": 1713454778.895877,
        "duration": 4.602184772491455,
        "processing": 4.142766952514648,
        "date_start": "2024-04-18T18:39:34+02:00",
        "date_finish": "2024-04-18T18:39:38+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.payment**
[`sale_order_payment`](../data-types.md#sale_order_payment) | Updated payment. In addition to the input fields, the object contains the payment `id`, the order `orderId`, the number `accountNumber`, the currency `currency`, the payment system name, type, and external code (`paySystemName`, `paySystemIsCash`, `paySystemXmlId`), and the dates of marking and of changing the responsible person (`dateMarked`, `dateResponsibleId`) ||
|| **time**
[`time`](../../data-types.md) | Information about the request processing time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "200640400001",
    "error_description":"payment does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200640400001` | `payment is not exists`

The payment being updated was not found ||
|| `200040300020` | `Access Denied`

Insufficient permissions to update the payment ||
|| `100` | Parameter `id` not specified ||
|| `100` | Parameter `fields` not specified ||
|| `0` | `Required fields: paySystemId`

`paySystemId` is not passed in `fields`, including when the `fields` object is empty ||
|| `0` | `Order total cannot be changed if the status is "Paid"`

A different amount is passed for a payment that is already paid ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-add.md)
- [{#T}](./sale-payment-get.md)
- [{#T}](./sale-payment-list.md)
- [{#T}](./sale-payment-delete.md)
- [{#T}](./sale-payment-get-fields.md)
