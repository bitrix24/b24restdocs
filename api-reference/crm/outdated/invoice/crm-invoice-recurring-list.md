# Get a List of Recurring Invoice Settings by Filter crm.invoice.recurring.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

This method returns a list of recurring invoice template settings based on the specified filter.

When querying, use the wildcard "*" to select all fields (excluding custom and multiple fields).

## Method Parameters

See the description of [list methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"INVOICE_ID":"ASC"},"filter":{">COUNTER_REPEAT":5},"select":["ID","INVOICE_ID","NEXT_EXECUTION","LAST_EXECUTION","SEND_BILL","IS_LIMIT"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.recurring.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"INVOICE_ID":"ASC"},"filter":{">COUNTER_REPEAT":5},"select":["ID","INVOICE_ID","NEXT_EXECUTION","LAST_EXECUTION","SEND_BILL","IS_LIMIT"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.recurring.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.
    
    try {
      const response = await $b24.callListMethod(
        'crm.invoice.recurring.list',
        {
          order: { "INVOICE_ID": "ASC" },
          filter: { ">COUNTER_REPEAT": 5 },
          select: [ "ID", "INVOICE_ID ", "NEXT_EXECUTION", "LAST_EXECUTION", "SEND_BILL", "IS_LIMIT" ]
        }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in chunks using an iterator. Use for large volumes of data for efficient memory consumption.
    
    try {
      const generator = $b24.fetchListMethod('crm.invoice.recurring.list', {
        order: { "INVOICE_ID": "ASC" },
        filter: { ">COUNTER_REPEAT": 5 },
        select: [ "ID", "INVOICE_ID ", "NEXT_EXECUTION", "LAST_EXECUTION", "SEND_BILL", "IS_LIMIT" ]
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.invoice.recurring.list', {
        order: { "INVOICE_ID": "ASC" },
        filter: { ">COUNTER_REPEAT": 5 },
        select: [ "ID", "INVOICE_ID ", "NEXT_EXECUTION", "LAST_EXECUTION", "SEND_BILL", "IS_LIMIT" ]
      }, 0)
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
                'crm.invoice.recurring.list',
                [
                    'order'  => ['INVOICE_ID' => 'ASC'],
                    'filter' => ['>COUNTER_REPEAT' => 5],
                    'select' => ['ID', 'INVOICE_ID', 'NEXT_EXECUTION', 'LAST_EXECUTION', 'SEND_BILL', 'IS_LIMIT'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
        if ($result->more()) {
            $result->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching recurring invoices: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.invoice.recurring.list",
        {
            order: { "INVOICE_ID": "ASC" },
            filter: { ">COUNTER_REPEAT": 5 },
            select: [ "ID", "INVOICE_ID ", "NEXT_EXECUTION", "LAST_EXECUTION", "SEND_BILL", "IS_LIMIT" ]
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
        'crm.invoice.recurring.list',
        [
            'order' => ['INVOICE_ID' => 'ASC'],
            'filter' => ['>COUNTER_REPEAT' => 5],
            'select' => ['ID', 'INVOICE_ID', 'NEXT_EXECUTION', 'LAST_EXECUTION', 'SEND_BILL', 'IS_LIMIT']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}