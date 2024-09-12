# Get the list of invoices crm.invoice.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method returns a list of invoices. It is an implementation of the list method for invoices.

When querying, use masks:

- "*" — to select all fields (excluding custom ones)
- "UF_*" — to select all custom fields.

The method does not return properties and line items of the invoice. To obtain properties and line items, use the method [crm.invoice.get](./crm-invoice-get.md).

## Method Parameters

See the description of [list methods](../../../how-to-call-rest-api/list-methods-pecularities.md).

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
 | Record filter. By default, all records are returned without filtering ||
|| **order**
 | Record sorting. Sorting is supported by the same fields as in the filter ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

This example outputs data to the console. If you need to display data differently, implement your own data handling for the results returned by `result.data()` and `result.error()`.

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"DATE_INSERT":"ASC"},"filter":{">PRICE":100},"select":["ID","ACCOUNT_NUMBER","ORDER_TOPIC","DATE_INSERT","STATUS_ID","PRICE","CURRENCY_ID"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"DATE_INSERT":"ASC"},"filter":{">PRICE":100},"select":["ID","ACCOUNT_NUMBER","ORDER_TOPIC","DATE_INSERT","STATUS_ID","PRICE","CURRENCY_ID"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.invoice.list",
        {
            "order": { "DATE_INSERT": "ASC" },
            "filter": { ">PRICE": 100 },
            "select": [ "ID", "ACCOUNT_NUMBER", "ORDER_TOPIC", "DATE_INSERT", "STATUS_ID", "PRICE", "CURRENCY_ID" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.invoice.list',
        [
            'order' => ['DATE_INSERT' => 'ASC'],
            'filter' => ['>PRICE' => 100],
            'select' => ['ID', 'ACCOUNT_NUMBER', 'ORDER_TOPIC', 'DATE_INSERT', 'STATUS_ID', 'PRICE', 'CURRENCY_ID']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Fields Returned by the Method

#|
|| **Field** / **Type** | **Description** | **Note** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier | Read-only ||
|| **ACCOUNT_NUMBER**
[`string`](../../../data-types.md) | Number | Required ||
|| **COMMENTS**
[`text`](../../../data-types.md) | Manager's comment | ||
|| **CREATED_BY**
[`integer`](../../../data-types.md) | Created by user | Read-only ||
|| **CURRENCY**
[`crm_currency`](../../../data-types.md) | Currency identifier | Read-only ||
|| **DATE_BILL**
[`date`](../../../data-types.md) | Invoice date | ||
|| **DATE_INSERT**
[`datetime`](../../../data-types.md) | Creation date | ||
|| **DATE_MARKED**
[`datetime`](../../../data-types.md) | Rejection date | Specified if the invoice is rejected ||
|| **DATE_PAY_BEFORE**
[`date`](../../../data-types.md) | Payment due date | ||
|| **DATE_PAYED**
[`datetime`](../../../data-types.md) | Date moved to paid status | Read-only ||
|| **DATE_STATUS**
[`datetime`](../../../data-types.md) | Date status changed | Read-only ||
|| **DATE_UPDATE**
[`datetime`](../../../data-types.md) | Date modified | Read-only ||
|| **EMP_PAYED_ID**
[`integer`](../../../data-types.md) | Identifier of the user who last marked the invoice as "paid" | Read-only ||
|| **EMP_STATUS_ID**
[`integer`](../../../data-types.md) | Identifier of the user who last changed the invoice status | Read-only ||
|| **LID**
[`integer`](../../../data-types.md) | Site identifier | Read-only ||
|| **IS_RECURRING**
[`char`](../../../data-types.md) | Flag for recurring deal template | ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code | ||
|| **ORDER_TOPIC**
[`string`](../../../data-types.md) | Subject | Required ||
|| **PAY_SYSTEM_ID**
[`integer`](../../../data-types.md) | Identifier of the printed form | Required ||
|| **PAY_VOUCHER_DATE**
[`date`](../../../data-types.md) | Payment date | Specified if the invoice is paid ||
|| **PAY_VOUCHER_NUM**
[`string`](../../../data-types.md) | Payment document number | Specified if the invoice is paid ||
|| **PAYED**
[`char`](../../../data-types.md) | Paid status | Read-only ||
|| **PERSON_TYPE_ID**
[`integer`](../../../data-types.md) | Payer type identifier | Required ||
|| **PRICE**
[`double`](../../../data-types.md) | Amount | Read-only ||
|| **REASON_MARKED**
[`string`](../../../data-types.md) | Status comment | Specified if the invoice is paid or rejected ||
|| **RESPONSIBLE_EMAIL**
[`string`](../../../data-types.md) | Responsible person's e-mail | Read-only ||
|| **RESPONSIBLE_ID**
[`integer`](../../../data-types.md) | Identifier of the responsible person | ||
|| **RESPONSIBLE_LAST_NAME**
[`string`](../../../data-types.md) | Last name of the responsible person | Read-only ||
|| **RESPONSIBLE_LOGIN**
[`string`](../../../data-types.md) | Login of the responsible person | Read-only ||
|| **RESPONSIBLE_NAME**
[`string`](../../../data-types.md) | First name of the responsible person | Read-only ||
|| **RESPONSIBLE_PERSONAL_PHOTO**
[`integer`](../../../data-types.md) | Identifier of the responsible person's photo | Read-only ||
|| **RESPONSIBLE_SECOND_NAME**
[`string`](../../../data-types.md) | Middle name of the responsible person | Read-only ||
|| **RESPONSIBLE_WORK_POSITION**
[`string`](../../../data-types.md) | Position of the responsible person | Read-only ||
|| **STATUS_ID**
[`crm_status`](../../../data-types.md) | Status identifier | Identifier of the "INVOICE_STATUS" reference ||
|| **TAX_VALUE**
[`double`](../../../data-types.md) | Tax amount | Read-only ||
|| **UF_COMPANY_ID**
[`integer`](../../../data-types.md) | Company identifier | Specified if the payer is a "Legal entity" ||
|| **UF_CONTACT_ID**
[`integer`](../../../data-types.md) | Contact identifier | Specified if the payer is an "Individual" or as a contact person for the company ||
|| **UF_MYCOMPANY_ID**
[`integer`](../../../data-types.md) | Identifier of your company | Specified as the company with the invoice details (link to the details is set separately) ||
|| **UF_DEAL_ID**
[`integer`](../../../data-types.md) | Identifier of the related deal | ||
|| **UF_QUOTE_ID**
[`integer`](../../../data-types.md) | Identifier of the related estimate | ||
|| **USER_DESCRIPTION**
[`string`](../../../data-types.md) | Comment | ||
|#