# Get a List of Custom Fields by Filter crm.invoice.userfield.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method returns a list of custom fields for invoices based on the filter.

## Parameters

#|
|| **Name**
`type` | **Description** ||
|| **order** | Sorting fields ||
|| **filter** | Filter fields  ||
|#

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"MANDATORY":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.userfield.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"MANDATORY":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.userfield.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.invoice.userfield.list',
        {
          order: {"SORT": "ASC"},
          filter: {"MANDATORY": "N"}
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.invoice.userfield.list', { order: {"SORT": "ASC"}, filter: {"MANDATORY": "N"} }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('crm.invoice.userfield.list', { order: {"SORT": "ASC"}, filter: {"MANDATORY": "N"} }, 0)
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
                'crm.invoice.userfield.list',
                [
                    'order' => ['SORT' => 'ASC'],
                    'filter' => ['MANDATORY' => 'N'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
        if ($response->getResponseData()->getMore()) {
            $response->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching invoice user fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.invoice.userfield.list",
        {
            order: {"SORT": "ASC"},
            filter: {"MANDATORY": "N"}
        },
        function (result)
        {
            if (result.error())
                console.error(result.error());
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
        'crm.invoice.userfield.list',
        [
            'order' => ['SORT' => 'ASC'],
            'filter' => ['MANDATORY' => 'N']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}