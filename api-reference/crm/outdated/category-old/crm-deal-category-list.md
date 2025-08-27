# Filter Deal Categories crm.dealcategory.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`crm.category.list`](../../universal/category/crm-category-list.md)

{% endnote %}

The method returns a list of deal categories based on the filter. It is an implementation of list methods for deal categories.

## Method Parameters

See the description of [list methods](../../../how-to-call-rest-api/list-methods-pecularities.md)

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"IS_LOCKED":"N"},"select":["ID","NAME","SORT"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.dealcategory.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"IS_LOCKED":"N"},"select":["ID","NAME","SORT"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.dealcategory.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.dealcategory.list',
        {
          order: { "SORT": "ASC" },
          filter: { "IS_LOCKED": "N" },
          select: [ "ID", "NAME", "SORT" ]
        }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative fetching using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.dealcategory.list', {
        order: { "SORT": "ASC" },
        filter: { "IS_LOCKED": "N" },
        select: [ "ID", "NAME", "SORT" ]
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.dealcategory.list', {
        order: { "SORT": "ASC" },
        filter: { "IS_LOCKED": "N" },
        select: [ "ID", "NAME", "SORT" ]
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
                'crm.dealcategory.list',
                [
                    'order'  => ['SORT' => 'ASC'],
                    'filter' => ['IS_LOCKED' => 'N'],
                    'select' => ['ID', 'NAME', 'SORT'],
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
        echo 'Error fetching deal categories: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.dealcategory.list",
        {
            order: { "SORT": "ASC" },
            filter: { "IS_LOCKED": "N" }, //Y - all categories, N - all categories except deleted. Deleted categories are not permanently removed from the database but only blocked.
            select: [ "ID", "NAME", "SORT" ]
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
        'crm.dealcategory.list',
        [
            'order' => ['SORT' => 'ASC'],
            'filter' => ['IS_LOCKED' => 'N'],
            'select' => ['ID', 'NAME', 'SORT']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}