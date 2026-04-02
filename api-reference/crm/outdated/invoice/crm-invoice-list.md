# Get a List of Invoices crm.invoice.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

This method returns a list of invoices. It is an implementation of the list method for invoices.

When querying, use the following masks:

- "*" — to select all fields (excluding custom fields)
- "UF_*" — to select all custom fields.

The method does not return properties and line items of the invoice. To obtain properties and line items, use the method [crm.invoice.get](./crm-invoice-get.md).

## Method Parameters

See the description of [list methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
 | Record filter. By default, all records are returned without filtering ||
|| **order**
 | Record sorting. Sorting is supported by the same fields as in the filter ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

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
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.
    
    try {
      const response = await $b24.callListMethod(
        'crm.invoice.list',
        {
          "order": { "DATE_INSERT": "ASC" },
          "filter": { ">PRICE": 100 },
          "select": [ "ID", "ACCOUNT_NUMBER", "ORDER_TOPIC", "DATE_INSERT", "STATUS_ID", "PRICE", "CURRENCY_ID" ]
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use for large volumes of data for efficient memory consumption.
    
    try {
      const generator = $b24.fetchListMethod('crm.invoice.list', { "order": { "DATE_INSERT": "ASC" }, "filter": { ">PRICE": 100 }, "select": [ "ID", "ACCOUNT_NUMBER", "ORDER_TOPIC", "DATE_INSERT", "STATUS_ID", "PRICE", "CURRENCY_ID" ] }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.invoice.list', { "order": { "DATE_INSERT": "ASC" }, "filter": { ">PRICE": 100 }, "select": [ "ID", "ACCOUNT_NUMBER", "ORDER_TOPIC", "DATE_INSERT", "STATUS_ID", "PRICE", "CURRENCY_ID" ] }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.invoice.list',
                [
                    'order' => ['DATE_INSERT' => 'ASC'],
                    'filter' => ['>PRICE' => 100],
                    'select' => ['ID', 'ACCOUNT_NUMBER', 'ORDER_TOPIC', 'DATE_INSERT', 'STATUS_ID', 'PRICE', 'CURRENCY_ID'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
            if ($result->more()) {
                $result->next();
            }
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching invoice list: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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
[`datetime`](../../../data-types.md) | Rejection date | Provided if the invoice is rejected ||
|| **DATE_PAY_BEFORE**
[`date`](../../../data-types.md) | Payment due date | ||
|| **DATE_PAYED**
[`datetime`](../../../data-types.md) | Date moved to paid status | Read-only ||
|| **DATE_STATUS**
[`datetime`](../../../data-types.md) | Date status changed | Read-only ||
|| **DATE_UPDATE**
[`datetime`](../../../data-types.md) | Date updated | Read-only ||
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
[`integer`](../../../data-types.md) | Identifier of the payment form | Required ||
|| **PAY_VOUCHER_DATE**
[`date`](../../../data-types.md) | Payment date | Provided if the invoice is paid ||
|| **PAY_VOUCHER_NUM**
[`string`](../../../data-types.md) | Payment document number | Provided if the invoice is paid ||
|| **PAYED**
[`char`](../../../data-types.md) | Payment status | Read-only ||
|| **PERSON_TYPE_ID**
[`integer`](../../../data-types.md) | Payer type identifier | Required ||
|| **PRICE**
[`double`](../../../data-types.md) | Amount | Read-only ||
|| **REASON_MARKED**
[`string`](../../../data-types.md) | Status comment | Provided if the invoice is paid or rejected ||
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
[`integer`](../../../data-types.md) | Company identifier | Provided if the payer is a "Legal entity" ||
|| **UF_CONTACT_ID**
[`integer`](../../../data-types.md) | Contact identifier | Provided if the payer is an "Individual" or as a contact person for the company ||
|| **UF_MYCOMPANY_ID**
[`integer`](../../../data-types.md) | Identifier of your company | Provided as the company with invoice details (binding to details is set separately) ||
|| **UF_DEAL_ID**
[`integer`](../../../data-types.md) | Identifier of the related deal | ||
|| **UF_QUOTE_ID**
[`integer`](../../../data-types.md) | Identifier of the related estimate | ||
|| **USER_DESCRIPTION**
[`string`](../../../data-types.md) | Comment | ||
|#