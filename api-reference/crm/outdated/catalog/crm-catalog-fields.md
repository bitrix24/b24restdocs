# Get Product Catalog Fields crm.catalog.fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [catalog.catalog.getFields](../../../catalog/catalog/catalog-catalog-get-fields.md).

{% endnote %}

This method returns the description of the product catalog fields.

No parameters required.

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.catalog.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/crm.catalog.fields?auth=**put_access_token_here**
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.catalog.fields',
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.catalog.fields',
                []
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
        echo 'Error fetching catalog fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.catalog.fields",
        {},
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

    $result = CRest::call(
        'crm.catalog.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **ID** 
[`integer`](../../../data-types.md) | Identifier. Read-only ||
|| **NAME** 
[`string`](../../../data-types.md) | Name ||
|| **ORIGINATOR_ID** 
[`string`](../../../data-types.md) | Data source identifier. Used only for linking to an external source ||
|| **ORIGIN_ID** 
[`string`](../../../data-types.md) | Identifier of the element in the data source. Used only for linking to an external source ||
|| **XML_ID** 
[`string`](../../../data-types.md) | Symbolic code. Read-only ||
|#