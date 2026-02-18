# Get the list of order properties crm.productrow.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`crm.item.productrow.list`](../../universal/product-rows/crm-item-productrow-list.md)

{% endnote %}

The method returns a list of product items based on the filter. It is an implementation of the list method for product items. The owner of the product items is determined by the required fields `OWNER_TYPE` and `OWNER_ID`, where `OWNER_TYPE` is a one-character code for the type ("D" - deal, "L" - lead), and `OWNER_ID` is the identifier.

## Method Parameters

See the description of [list methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md)

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"OWNER_TYPE":"D","OWNER_ID":"1"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.productrow.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"OWNER_TYPE":"D","OWNER_ID":"1"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.productrow.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    var ownerType = prompt("Enter owner type (D, L)");
    var ownerId = prompt("Enter owner ID");
    
    try {
      const response = await $b24.callListMethod(
        'crm.productrow.list',
        {
          filter: {
            "OWNER_TYPE": ownerType,
            "OWNER_ID": ownerId
          }
        },
        (progress) => { 
          if(progress.error())
            console.error(progress.error());
          else
          {
            console.dir(progress.data());
            if(progress.more())
              progress.next();
          }
        }
      );
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    var ownerType = prompt("Enter owner type (D, L)");
    var ownerId = prompt("Enter owner ID");
    
    try {
      const generator = $b24.fetchListMethod('crm.productrow.list', {
        filter: {
          "OWNER_TYPE": ownerType,
          "OWNER_ID": ownerId
        }
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { 
          console.log('Entity:', entity);
        }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    var ownerType = prompt("Enter owner type (D, L)");
    var ownerId = prompt("Enter owner ID");
    
    try {
      const response = await $b24.callMethod('crm.productrow.list', {
        filter: {
          "OWNER_TYPE": ownerType,
          "OWNER_ID": ownerId
        }
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { 
        console.log('Entity:', entity);
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    $ownerType = readline("Enter owner type (D, L): ");
    $ownerId = readline("Enter owner ID: ");
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.productrow.list',
                [
                    'filter' => [
                        'OWNER_TYPE' => $ownerType,
                        'OWNER_ID'   => $ownerId,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Data: ' . print_r($result->data(), true);
            if ($result->more()) {
                $result->next();
            }
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var ownerType = prompt("Enter owner type (D, L)");
    var ownerId = prompt("Enter owner ID");
    BX24.callMethod(
        "crm.productrow.list",
        {
            filter:
            {
                "OWNER_TYPE": ownerType,
                "OWNER_ID": ownerId
            }
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

    $ownerType = 'D'; // Replace 'D' with the desired owner type
    $ownerId = 1; // Replace 1 with the actual owner ID

    $result = CRest::call(
        'crm.productrow.list',
        [
            'filter' =>
            [
                'OWNER_TYPE' => $ownerType,
                'OWNER_ID' => $ownerId
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

