# Get a List of Sections from crm.productsection.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [catalog.section.list](../../../catalog/section/catalog-section-list.md).

{% endnote %}

The method `crm.productsection.list` returns a list of product sections based on a filter. It is an implementation of the list method for product sections. It is expected that the filter will specify the `CATALOG_ID` parameter. Otherwise, sections will be selected from the default catalog.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array containing the list of fields to be selected ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected items ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected items  ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

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
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.
    
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
    
    // fetchListMethod: Retrieves data in chunks using an iterator. Use for large volumes of data for efficient memory consumption.
    
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
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
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