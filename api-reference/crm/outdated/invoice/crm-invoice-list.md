# Get the list of invoices crm.invoice.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method returns a list of invoices. It is an implementation of the list method for invoices.

When querying, use masks:

- "*" — to select all fields (excluding custom fields)
- "UF_*" — to select all custom fields.

The method does not return properties and line items of the invoice. To obtain properties and line items, use the method [crm.invoice.get](./crm-invoice-get.md).

## Method parameters

See the description of [list methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
 | Record filter. By default, all records are returned without filtering ||
|| **order**
 | Record sorting. Sorting is supported by the same fields as in the filter ||
|#

## Code examples

{% include [Note on examples](../../../../_includes/examples.md) %}

The example outputs data to the console. If you need to output data differently, implement your own data handling based on the results returned by `result.data()` and `result.error()` calls.

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
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
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
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.invoice.list', { "order": { "DATE_INSERT": "ASC" }, "filter": { ">PRICE": 100 }, "select": [ "ID", "ACCOUNT_NUMBER", "ORDER_TOPIC", "DATE_INSERT", "STATUS_ID", "PRICE", "CURRENCY_ID" ] }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
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

## Fields returned by the method

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
[`datetime`](../../../data-types.md) | Date changed to paid status | Read-only ||
|| **DATE_STATUS**
[`datetime`](../../../data-types.md) | Date of status change | Read-only ||
|| **DATE_UPDATE**
[`datetime`](../../../data-types.md) | Date of update | Read-only ||
|| **EMP_PAYED_ID**
[`integer`](../../../data-types.md) | Identifier of the user who last marked the invoice as paid | Read-only ||
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
[`integer`](../../../data-types.md) | Contact identifier | Specified if the payer is a "Natural person" or as a contact person of the company ||
|| **UF_MYCOMPANY_ID**
[`integer`](../../../data-types.md) | Identifier of your company | Specified as the company with invoice details (link to details is set separately) ||
|| **UF_DEAL_ID**
[`integer`](../../../data-types.md) | Identifier of the related deal | ||
|| **UF_QUOTE_ID**
[`integer`](../../../data-types.md) | Identifier of the related estimate | ||
|| **USER_DESCRIPTION**
[`string`](../../../data-types.md) | Comment | ||
|#