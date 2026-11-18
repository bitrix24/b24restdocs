# Get the list of payments sale.payment.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: store manager

The method `sale.payment.list` retrieves a list of payments.

## Method Parameters

All parameters are optional.

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select.

If not provided or an empty array is passed, all available payment fields will be selected.

Possible values for the array elements correspond to the fields of the [sale_order_payment](../data-types.md#sale_order_payment) object.
 ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected payments in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_payment](../data-types.md#sale_order_payment) object. The method skips a condition with an unknown field without an error, so a typo in a field name widens the selection.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `=` — equals (works with arrays as well)
- `@` — in a list of values (IN), the filter value is an array
- `%` — LIKE, searches for a substring in any position of the string. The % symbol in the filter value does not need to be passed
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be passed. The search goes from both sides
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol needs to be passed in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol needs to be passed in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)

 ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected payments in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_payment](../data-types.md#sale_order_payment) object.

Possible values for order:

- `asc` — in ascending order
- `desc` — in descending order

By default, payments are sorted by `id` in ascending order
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, pass the value `50`. For the third page, pass `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` — the number of the desired page.
 
If you pass `-1`, the method returns the first 50 records without counting the total number: the `total` and `next` fields are not included in the response. This makes the request faster on large selections ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["paySystemXmlId","paySystemIsCash","accountNumber","id","orderId","paid","datePaid","empPaidId","paySystemId","psStatus","psStatusCode","psStatusDescription","psStatusMessage","psSum","psCurrency","psResponseDate","payVoucherNum","payVoucherDate","datePayBefore","dateBill","xmlId","sum","currency","paySystemName","companyId","payReturnNum","priceCod","payReturnDate","empReturnId","payReturnComment","responsibleId","empResponsibleId","dateResponsibleId","isReturn","comments","updated1c","id1c","version1c","externalPayment","psInvoiceId","marked","reasonMarked","dateMarked","empMarkedId"],"filter":{"<id":10,"paid":"N"},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.payment.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["paySystemXmlId","paySystemIsCash","accountNumber","id","orderId","paid","datePaid","empPaidId","paySystemId","psStatus","psStatusCode","psStatusDescription","psStatusMessage","psSum","psCurrency","psResponseDate","payVoucherNum","payVoucherDate","datePayBefore","dateBill","xmlId","sum","currency","paySystemName","companyId","payReturnNum","priceCod","payReturnDate","empReturnId","payReturnComment","responsibleId","empResponsibleId","dateResponsibleId","isReturn","comments","updated1c","id1c","version1c","externalPayment","psInvoiceId","marked","reasonMarked","dateMarked","empMarkedId"],"filter":{"<id":10,"paid":"N"},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.payment.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SalePaymentListResult = {
      payments: {
        accountNumber: string
        comments: string
        companyId: number | null
        currency: string
        dateBill: ISODate | null
        dateMarked: ISODate | null
        datePaid: ISODate | null
        datePayBefore: ISODate | null
        dateResponsibleId: ISODate | null
        empMarkedId: number | null
        empPaidId: number | null
        empResponsibleId: number | null
        empReturnId: number | null
        externalPayment: string
        id: number
        id1c: string
        isReturn: string
        marked: string
        orderId: number
        paid: string
        payReturnComment: string
        payReturnDate: ISODate | null
        payReturnNum: string
        paySystemId: number
        paySystemIsCash: string
        paySystemName: string
        paySystemXmlId: string
        payVoucherDate: ISODate | null
        payVoucherNum: string
        priceCod: number | null
        psCurrency: string
        psInvoiceId: string
        psResponseDate: ISODate | null
        psStatus: string
        psStatusCode: string
        psStatusDescription: string
        psStatusMessage: string
        psSum: number | null
        reasonMarked: string
        responsibleId: number | null
        sum: number
        updated1c: string
        version1c: string
        xmlId: string
      }[]
    }

    try {
      // sale.payment.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<SalePaymentListResult>({
        method: 'sale.payment.list',
        params: {
          select: [
            'paySystemXmlId',
            'paySystemIsCash',
            'accountNumber',
            'id',
            'orderId',
            'paid',
            'datePaid',
            'empPaidId',
            'paySystemId',
            'psStatus',
            'psStatusCode',
            'psStatusDescription',
            'psStatusMessage',
            'psSum',
            'psCurrency',
            'psResponseDate',
            'payVoucherNum',
            'payVoucherDate',
            'datePayBefore',
            'dateBill',
            'xmlId',
            'sum',
            'currency',
            'paySystemName',
            'companyId',
            'payReturnNum',
            'priceCod',
            'payReturnDate',
            'empReturnId',
            'payReturnComment',
            'responsibleId',
            'empResponsibleId',
            'dateResponsibleId',
            'isReturn',
            'comments',
            'updated1c',
            'id1c',
            'version1c',
            'externalPayment',
            'psInvoiceId',
            'marked',
            'reasonMarked',
            'dateMarked',
            'empMarkedId',
          ],
          filter: {
            '<id': 10,
            paid: 'N',
          },
          order: {
            id: 'desc',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Payments on this page:', result.payments.length, result.payments)
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
      async function fetchPaymentList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.payment.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.payment.list',
            params: {
              select: [
                'paySystemXmlId',
                'paySystemIsCash',
                'accountNumber',
                'id',
                'orderId',
                'paid',
                'datePaid',
                'empPaidId',
                'paySystemId',
                'psStatus',
                'psStatusCode',
                'psStatusDescription',
                'psStatusMessage',
                'psSum',
                'psCurrency',
                'psResponseDate',
                'payVoucherNum',
                'payVoucherDate',
                'datePayBefore',
                'dateBill',
                'xmlId',
                'sum',
                'currency',
                'paySystemName',
                'companyId',
                'payReturnNum',
                'priceCod',
                'payReturnDate',
                'empReturnId',
                'payReturnComment',
                'responsibleId',
                'empResponsibleId',
                'dateResponsibleId',
                'isReturn',
                'comments',
                'updated1c',
                'id1c',
                'version1c',
                'externalPayment',
                'psInvoiceId',
                'marked',
                'reasonMarked',
                'dateMarked',
                'empMarkedId',
              ],
              filter: {
                '<id': 10,
                paid: 'N',
              },
              order: {
                id: 'desc',
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
          console.info('Payments on this page:', result.payments.length, result.payments)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchPaymentList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.sale.payment.list(
            select=[
                "paySystemXmlId",
                "paySystemIsCash",
                "accountNumber",
                "id",
                "orderId",
                "paid",
                "datePaid",
                "empPaidId",
                "paySystemId",
                "psStatus",
                "psStatusCode",
                "psStatusDescription",
                "psStatusMessage",
                "psSum",
                "psCurrency",
                "psResponseDate",
                "payVoucherNum",
                "payVoucherDate",
                "datePayBefore",
                "dateBill",
                "xmlId",
                "sum",
                "currency",
                "paySystemName",
                "companyId",
                "payReturnNum",
                "priceCod",
                "payReturnDate",
                "empReturnId",
                "payReturnComment",
                "responsibleId",
                "empResponsibleId",
                "dateResponsibleId",
                "isReturn",
                "comments",
                "updated1c",
                "id1c",
                "version1c",
                "externalPayment",
                "psInvoiceId",
                "marked",
                "reasonMarked",
                "dateMarked",
                "empMarkedId",
            ],
            filter={
                "<id": 10,
                "paid": "N",
            },
            order={
                "id": "desc",
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
                'sale.payment.list',
                [
                    'select' => [
                        'paySystemXmlId',
                        'paySystemIsCash',
                        'accountNumber',
                        'id',
                        'orderId',
                        'paid',
                        'datePaid',
                        'empPaidId',
                        'paySystemId',
                        'psStatus',
                        'psStatusCode',
                        'psStatusDescription',
                        'psStatusMessage',
                        'psSum',
                        'psCurrency',
                        'psResponseDate',
                        'payVoucherNum',
                        'payVoucherDate',
                        'datePayBefore',
                        'dateBill',
                        'xmlId',
                        'sum',
                        'currency',
                        'paySystemName',
                        'companyId',
                        'payReturnNum',
                        'priceCod',
                        'payReturnDate',
                        'empReturnId',
                        'payReturnComment',
                        'responsibleId',
                        'empResponsibleId',
                        'dateResponsibleId',
                        'isReturn',
                        'comments',
                        'updated1c',
                        'id1c',
                        'version1c',
                        'externalPayment',
                        'psInvoiceId',
                        'marked',
                        'reasonMarked',
                        'dateMarked',
                        'empMarkedId',
                    ],
                    'filter' => [
                        '<id'           => 10,
                        'paid'         => 'N',
                    ],
                    'order' => [
                        'id' => 'desc',
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
        "sale.payment.list",
        {
            "select": [
                "paySystemXmlId",
                "paySystemIsCash",
                "accountNumber",
                "id",
                "orderId",
                "paid",
                "datePaid",
                "empPaidId",
                "paySystemId",
                "psStatus",
                "psStatusCode",
                "psStatusDescription",
                "psStatusMessage",
                "psSum",
                "psCurrency",
                "psResponseDate",
                "payVoucherNum",
                "payVoucherDate",
                "datePayBefore",
                "dateBill",
                "xmlId",
                "sum",
                "currency",
                "paySystemName",
                "companyId",
                "payReturnNum",
                "priceCod",
                "payReturnDate",
                "empReturnId",
                "payReturnComment",
                "responsibleId",
                "empResponsibleId",
                "dateResponsibleId",
                "isReturn",
                "comments",
                "updated1c",
                "id1c",
                "version1c",
                "externalPayment",
                "psInvoiceId",
                "marked",
                "reasonMarked",
                "dateMarked",
                "empMarkedId",
            ],
            "filter": {
                "<id": 10,
                "paid": "N",
            },
            "order": {
                "id": "desc",
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
        'sale.payment.list',
        [
            'select' => [
                "paySystemXmlId",
                "paySystemIsCash",
                "accountNumber",
                "id",
                "orderId",
                "paid",
                "datePaid",
                "empPaidId",
                "paySystemId",
                "psStatus",
                "psStatusCode",
                "psStatusDescription",
                "psStatusMessage",
                "psSum",
                "psCurrency",
                "psResponseDate",
                "payVoucherNum",
                "payVoucherDate",
                "datePayBefore",
                "dateBill",
                "xmlId",
                "sum",
                "currency",
                "paySystemName",
                "companyId",
                "payReturnNum",
                "priceCod",
                "payReturnDate",
                "empReturnId",
                "payReturnComment",
                "responsibleId",
                "empResponsibleId",
                "dateResponsibleId",
                "isReturn",
                "comments",
                "updated1c",
                "id1c",
                "version1c",
                "externalPayment",
                "psInvoiceId",
                "marked",
                "reasonMarked",
                "dateMarked",
                "empMarkedId",
            ],
            'filter' => [
                "<id" => 10,
                "paid" => "N",
            ],
            'order' => [
                "id" => "desc",
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
    res, err := client.Core().Call(ctx, "sale.payment.list", b24.Params{
    	"select": []string{"paySystemXmlId", "paySystemIsCash", "accountNumber", "id", "orderId", "paid", "datePaid", "empPaidId", "paySystemId", "psStatus", "psStatusCode", "psStatusDescription", "psStatusMessage", "psSum", "psCurrency", "psResponseDate", "payVoucherNum", "payVoucherDate", "datePayBefore", "dateBill", "xmlId", "sum", "currency", "paySystemName", "companyId", "payReturnNum", "priceCod", "payReturnDate", "empReturnId", "payReturnComment", "responsibleId", "empResponsibleId", "dateResponsibleId", "isReturn", "comments", "updated1c", "id1c", "version1c", "externalPayment", "psInvoiceId", "marked", "reasonMarked", "dateMarked", "empMarkedId"},
    	"filter": b24.Params{
    		"<id":           10,
        "paid":          "N",
    	},
    	"order": b24.Params{
    		"id": "desc",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sale.payment.list: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "payments": [
            {
                "accountNumber": "163\/1",
                "comments": "",
                "companyId": null,
                "currency": "USD",
                "dateBill": "2022-10-14T17:10:00+02:00",
                "dateMarked": null,
                "datePaid": null,
                "datePayBefore": null,
                "dateResponsibleId": "2022-10-14T17:10:00+02:00",
                "empMarkedId": null,
                "empPaidId": null,
                "empResponsibleId": 1,
                "empReturnId": null,
                "externalPayment": "N",
                "id": 9,
                "id1c": "",
                "isReturn": "N",
                "marked": "N",
                "orderId": 7,
                "paid": "N",
                "payReturnComment": "",
                "payReturnDate": null,
                "payReturnNum": "",
                "paySystemId": 6,
                "paySystemIsCash": "Y",
                "paySystemName": "Cash",
                "paySystemXmlId": "bx_64134ba550ffa",
                "payVoucherDate": null,
                "payVoucherNum": "",
                "priceCod": 0,
                "psCurrency": "",
                "psInvoiceId": "",
                "psResponseDate": null,
                "psStatus": "",
                "psStatusCode": "",
                "psStatusDescription": "",
                "psStatusMessage": "",
                "psSum": null,
                "reasonMarked": "",
                "responsibleId": 1,
                "sum": 1176,
                "updated1c": "N",
                "version1c": "",
                "xmlId": "bx_634989d809dc8"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1713451909.778956,
        "finish": 1713451910.23781,
        "duration": 0.45885396003723145,
        "processing": 0.09081101417541504,
        "date_start": "2024-04-18T17:51:49+02:00",
        "date_finish": "2024-04-18T17:51:50+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **payments**
[`sale_order_payment[]`](../data-types.md#sale_order_payment) | Array of the selected payments inside `result`. The set of fields is defined by `select`. If nothing is found, the array is empty ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found. Not returned when `start` = `-1` ||
|| **next**
[`integer`](../../data-types.md) | The value to pass in `start` to retrieve the next page. Returned if there is a next page ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"200040300010",
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | `Access Denied`

Insufficient permissions to read payments ||
|| `100` | `Invalid order "<VALUE>"`

A sort direction other than `asc` and `desc` is passed in `order` ||
|| `100` | `Order must be a string`

The sort direction in `order` is not passed as a string ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-payment-add.md)
- [{#T}](./sale-payment-update.md)
- [{#T}](./sale-payment-get.md)
- [{#T}](./sale-payment-delete.md)
- [{#T}](./sale-payment-get-fields.md)

