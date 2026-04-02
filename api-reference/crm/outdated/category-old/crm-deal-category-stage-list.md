# Get a List of Deal Stages for the crm.dealcategory.stage.list Endpoint

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.category.*](../../universal/category/index.md).

{% endnote %}

This method returns a list of deal stages for the direction specified by the identifier. It is equivalent to calling the [`crm.status.list`](../../status/crm-status-list.md) method with the `ENTITY_ID` parameter set to the result of the [`crm.dealcategory.status`](crm-deal-category-status.md) call.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md)| Identifier of the direction. If `id = 0` or nothing [is specified](*quotes), it will return the statuses of the "default" direction. If `id > 0` [for a non-existent direction](*id), it returns nothing ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"1"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.dealcategory.stage.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"1","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.dealcategory.stage.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small datasets (< 1000 items) due to high memory load.
    
    var id = prompt("Enter ID");
    try {
      const response = await $b24.callListMethod(
        'crm.dealcategory.stage.list',
        { id: id },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use for large datasets for efficient memory consumption.
    
    var id = prompt("Enter ID");
    try {
      const generator = $b24.fetchListMethod('crm.dealcategory.stage.list', { id: id }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    var id = prompt("Enter ID");
    try {
      const response = await $b24.callMethod('crm.dealcategory.stage.list', { id: id }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    $id = $_POST['id'];
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.dealcategory.stage.list',
                [
                    'id' => $id,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.dealcategory.stage.list",
        { id: id },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $id = 1; // Replace 1 with the actual ID

    $result = CRest::call(
        'crm.dealcategory.stage.list',
        [
            'id' => $id
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}


[*quotes]: Empty quotes or not passing the parameter at all

[*id]: For example, specifying id = 10, but there is no direction with id=10 in the system