# Get the list of sections crm.productsection.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.productsection.list` returns a list of product sections based on a filter. It is an implementation of a list method for product sections. It is expected that the filter will define the `CATALOG_ID` parameter. Otherwise, sections will be selected from the default catalog.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | The array contains a list of fields to select ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected items ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected items  ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    catalogId=$(prompt "Enter the catalog ID")

    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "order": { "NAME": "ASC" },
        "filter": { "CATALOG_ID": '"$catalogId"' },
        "select": [ "ID", "NAME" ]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.productsection.list
    ```

- cURL (OAuth)

    ```curl
    catalogId=$(prompt "Enter the catalog ID")

    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "order": { "NAME": "ASC" },
        "filter": { "CATALOG_ID": '"$catalogId"' },
        "select": [ "ID", "NAME" ],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/crm.productsection.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    var catalogId = prompt("Enter the catalog ID");
    try {
      const response = await $b24.callListMethod(
        'crm.productsection.list',
        {
          order: { "NAME": "ASC" },
          filter: { "CATALOG_ID": catalogId },
          select: [ "ID", "NAME" ]
        }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    var catalogId = prompt("Enter the catalog ID");
    try {
      const generator = $b24.fetchListMethod('crm.productsection.list', {
        order: { "NAME": "ASC" },
        filter: { "CATALOG_ID": catalogId },
        select: [ "ID", "NAME" ]
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    var catalogId = prompt("Enter the catalog ID");
    try {
      const response = await $b24.callMethod('crm.productsection.list', {
        order: { "NAME": "ASC" },
        filter: { "CATALOG_ID": catalogId },
        select: [ "ID", "NAME" ]
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    $catalogId = readline("Enter the catalog ID");
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.productsection.list',
                [
                    'order'  => ['NAME' => 'ASC'],
                    'filter' => ['CATALOG_ID' => $catalogId],
                    'select' => ['ID', 'NAME'],
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
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var catalogId = prompt("Enter the catalog ID");
    BX24.callMethod(
        "crm.productsection.list",
        {
            order: { "NAME": "ASC" },
            filter: { "CATALOG_ID": catalogId },
            select: [ "ID", "NAME" ]
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

    $catalogId = readline("Enter the catalog ID: ");

    $result = CRest::call(
        'crm.productsection.list',
        [
            'order' => [ 'NAME' => 'ASC' ],
            'filter' => [ 'CATALOG_ID' => $catalogId ],
            'select' => [ 'ID', 'NAME' ]
        ]
    );

    if (isset($result['error'])) {
        echo "Error: " . $result['error_description'] . "\n";
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ``` 

{% endlist %}