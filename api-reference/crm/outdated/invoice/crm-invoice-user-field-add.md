# Create a Custom Field for Invoices crm.invoice.userfield.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

This method creates a new custom field for invoices.

There is a system limitation on the field name - 20 characters. The custom field name is always prefixed with `UF_CRM_`, meaning the actual length of the name is 13 characters.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields** | A set of fields - an array of the form `array("field"=>"value"[, ...])`, containing the description of the custom field. A complete description of the fields can be obtained by calling the method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md). ||
|| **LIST** | Contains a set of list values for custom fields of type List. This is specified when creating/updating the field. Each value is an array with the following fields: 
- **VALUE** - the value of the list item. This field is required when creating a new item.  
- **SORT** - sorting order. 
- **DEF** - if set to Y, the list item is the default value. For multiple fields, multiple DEF=Y values are allowed. For non-multiple fields, the first one will be considered default.  
- **XML_ID** - external code of the value. This parameter is only considered when updating existing list item values.
- **ID** - identifier of the value. If specified, it is considered an update of an existing list item value, not the creation of a new one. This is only relevant when calling the `*.userfield.update` methods.
- **DEL** - if set to Y, the existing list item will be deleted. This is applied if the ID parameter is filled. ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"%NAME":"Estimate"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.paysystem.list  
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"%NAME":"Estimate"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.paysystem.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small datasets (< 1000 items) due to high memory load.
    
    try {
      const response = await $b24.callListMethod(
        'crm.paysystem.list',
        {
          order: {"SORT": "ASC"},
          filter: {
            "%NAME": "Estimate",
          }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in chunks using an iterator. Use for large datasets for efficient memory consumption.
    
    try {
      const generator = $b24.fetchListMethod('crm.paysystem.list', {
        order: {"SORT": "ASC"},
        filter: {
          "%NAME": "Estimate",
        }
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.paysystem.list', {
        order: {"SORT": "ASC"},
        filter: {
          "%NAME": "Estimate",
        }
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
                'crm.paysystem.list',
                [
                    'order' => ['SORT' => 'ASC'],
                    'filter' => [
                        '%NAME' => 'Estimate',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        if ($response->more()) {
            $response->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing payment systems: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.paysystem.list", {
            order: {"SORT": "ASC"},
            filter: {
                "%NAME": "Estimate",
            }
        },
        function (result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.dir(result.data());
                if (result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.paysystem.list',
        [
            'order' => ['SORT' => 'ASC'],
            'filter' => ['%NAME' => 'Estimate']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}