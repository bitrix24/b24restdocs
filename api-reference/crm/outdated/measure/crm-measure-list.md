# Get the List of Measurement Units crm.measure.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [catalog.measure.list](../../../catalog/measure/catalog-measure-list.md).

{% endnote %}

This method returns a list of measurement units.

See the description of [list methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"IS_DEFAULT":"Y"},"select":["ID","CODE","STAGE_ID","SYMBOL_INTL","SYMBOL_INTL"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.measure.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"IS_DEFAULT":"Y"},"select":["ID","CODE","STAGE_ID","SYMBOL_INTL","SYMBOL_INTL"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.measure.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small datasets (< 1000 items) due to high memory load.
    
    try {
      const response = await $b24.callListMethod(
        'crm.measure.list',
        {
          order: {"ID": "ASC"},
          filter: {"IS_DEFAULT": "Y"},
          select: ["ID", "CODE", "STAGE_ID", "SYMBOL_INTL", "SYMBOL_INTL"]
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
      const generator = $b24.fetchListMethod('crm.measure.list', {
        order: {"ID": "ASC"},
        filter: {"IS_DEFAULT": "Y"},
        select: ["ID", "CODE", "STAGE_ID", "SYMBOL_INTL", "SYMBOL_INTL"]
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.measure.list', {
        order: {"ID": "ASC"},
        filter: {"IS_DEFAULT": "Y"},
        select: ["ID", "CODE", "STAGE_ID", "SYMBOL_INTL", "SYMBOL_INTL"]
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
                'crm.measure.list',
                [
                    'order'  => ['ID' => 'ASC'],
                    'filter' => ['IS_DEFAULT' => 'Y'],
                    'select' => ['ID', 'CODE', 'STAGE_ID', 'SYMBOL_INTL', 'SYMBOL_INTL'],
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
        echo 'Error fetching measure list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.measure.list",
        {
            order: {"ID": "ASC"},
            filter: {"IS_DEFAULT": "Y"},
            select: ["ID", "CODE", "STAGE_ID", "SYMBOL_INTL", "SYMBOL_INTL"]
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
        'crm.measure.list',
        [
            'order' => ['ID' => 'ASC'],
            'filter' => ['IS_DEFAULT' => 'Y'],
            'select' => ['ID', 'CODE', 'STAGE_ID', 'SYMBOL_INTL', 'SYMBOL_INTL']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./crm-measure-add.md)
- [{#T}](./crm-measure-update.md)
- [{#T}](./crm-measure-get.md)
- [{#T}](./crm-measure-delete.md)
- [{#T}](./crm-measure-fields.md)