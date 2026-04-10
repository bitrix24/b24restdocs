# Get Store Document Types catalog.enum.getStoreDocumentTypes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `catalog.enum.getStoreDocumentTypes` returns the types of store accounting documents available for REST.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.enum.getStoreDocumentTypes
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.enum.getStoreDocumentTypes
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.enum.getStoreDocumentTypes',
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.enum.getStoreDocumentTypes',
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
        echo 'Error getting store document types: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.enum.getStoreDocumentTypes',
        {},
        function(result) {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.enum.getStoreDocumentTypes',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "enum": [
        {
            "id": "A",
            "name": "Goods Receipt"
        },
        {
            "id": "S",
            "name": "Goods Acquisition"
        },
        {
            "id": "M",
            "name": "Goods Transfer Between Warehouses"
        },
        {
            "id": "R",
            "name": "Goods Return"
        },
        {
            "id": "D",
            "name": "Goods Write-off"
        }
        ]
    },
    "time": {
        "start": 1774268098,
        "finish": 1774268098.509955,
        "duration": 0.5099549293518066,
        "processing": 0,
        "date_start": "2026-03-23T15:14:58+01:00",
        "date_finish": "2026-03-23T15:14:58+01:00",
        "operating_reset_at": 1774268698,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **enum**
[`catalog_enum[]`](../data-types.md#catalog_enum) | Array of enumeration elements for store accounting document types ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-enum-get-round-types.md)