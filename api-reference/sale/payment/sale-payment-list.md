# Get the list of payments sale.payment.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of payments.

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

Possible values for `field` correspond to the fields of the [sale_order_payment](../data-types.md#sale_order_payment) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be passed. The search goes from both sides.
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
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, the value `50` must be passed. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["paySystemXmlId","paySystemIsCash","accountNumber","id","orderId","paid","datePaid","empPaidId","paySystemId","psStatus","psStatusCode","psStatusDescription","psStatusMessage","psSum","psCurrency","psResponseDate","payVoucherNum","payVoucherDate","datePayBefore","dateBill","xmlId","sum","currency","paySystemName","companyId","payReturnNum","priceCod","payReturnDate","empReturnId","payReturnComment","responsibleId","empResponsibleId","dateResponsibleId","isReturn","comments","updated1c","id1c","version1c","externalPayment","psInvoiceId","marked","reasonMarked","dateMarked","empMarkedId"],"filter":{"<id":10,"@personTypeId":[3,4],"payed":"N"},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.payment.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["paySystemXmlId","paySystemIsCash","accountNumber","id","orderId","paid","datePaid","empPaidId","paySystemId","psStatus","psStatusCode","psStatusDescription","psStatusMessage","psSum","psCurrency","psResponseDate","payVoucherNum","payVoucherDate","datePayBefore","dateBill","xmlId","sum","currency","paySystemName","companyId","payReturnNum","priceCod","payReturnDate","empReturnId","payReturnComment","responsibleId","empResponsibleId","dateResponsibleId","isReturn","comments","updated1c","id1c","version1c","externalPayment","psInvoiceId","marked","reasonMarked","dateMarked","empMarkedId"],"filter":{"<id":10,"@personTypeId":[3,4],"payed":"N"},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.payment.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'sale.payment.list',
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
            "@personTypeId": [3, 4],
            "payed": "N",
          },
          "order": {
            "id": "desc",
          }
        },
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('sale.payment.list', {
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
          "@personTypeId": [3, 4],
          "payed": "N",
        },
        "order": {
          "id": "desc",
        }
      }, 'id');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('sale.payment.list', {
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
          "@personTypeId": [3, 4],
          "payed": "N",
        },
        "order": {
          "id": "desc",
        }
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
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
                        '@personTypeId' => [3, 4],
                        'payed'         => 'N',
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
                "@personTypeId": [3, 4],
                "payed": "N",
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
                "@personTypeId" => [3, 4],
                "payed" => "N",
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
                "priceCod": "0.000000",
                "psCurrency": "",
                "psInvoiceId": null,
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
            },
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
[`sale_order_payment[]`](../data-types.md) | An array of objects containing information about the selected payments ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read payments ||
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