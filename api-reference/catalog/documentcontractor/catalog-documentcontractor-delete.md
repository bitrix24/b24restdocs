# Remove Vendor Binding from Document catalog.documentcontractor.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the following permissions:
> — "View" and "Create and Edit" for the document type "Incoming",
> — "View Inventory Accounting Section"
> — "View Product Catalog"    

The method `catalog.documentcontractor.delete` removes the vendor from the inventory accounting document.

## Method Parameters  

{% include [Note on required parameters](../../../_includes/required.md) %}  

#|
|| **Name**
`type` | **Description** || 
|| **id***
[`catalog_documentcontractor.id`](../data-types.md#catalog_documentcontractor) | Identifier for the vendor binding to the document. It can be obtained using the method [catalog.documentcontractor.list](./catalog-documentcontractor-list.md) ||  
|#  

## Code Examples  

{% include [Note on examples](../../../_includes/examples.md) %}  

{% list tabs %}

- cURL (Webhook)

    ```bash 
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.documentcontractor.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.documentcontractor.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'catalog.documentcontractor.delete',
        params: {
          id: 42,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Contractor binding deleted:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function deleteDocumentContractor() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.documentcontractor.delete',
            params: {
              id: 42,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Contractor binding deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteDocumentContractor)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.documentcontractor.delete',
                [
                    'id' => 42,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result === true) {
            echo 'Contractor binding deleted';
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting contractor binding: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.documentcontractor.delete',
        { id: 42 },
        function(result)
        {
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
        'catalog.documentcontractor.delete',
        [
            'id' => 42,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}    

## Response Handling  

HTTP Code: **200**

```json
{
     "result": true,
     "time": {
         "start": 1761908531,
         "finish": 1761908531.935914,
         "duration": 0.9359140396118164,
         "processing": 0,
         "date_start": "2025-10-31T14:02:11+02:00",
         "date_finish": "2025-10-31T14:02:11+02:00",
         "operating_reset_at": 1761909131,
         "operating": 0
    }
}
```

## Returned Data  

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md)| Root element of the response, contains `true` if the binding was successfully deleted.  
If the response contains `null` — the binding was not found or the user does not have permission to modify the document ||  
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time || 
|#  

## Error Handling  

{% include notitle [error handling](../../../_includes/error-info.md) %}  

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "Binding was not found"
}
```

### Possible Error Codes  

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Binding was not found | The specified binding identifier does not exist ||  
|| `0` | Access denied | Insufficient permissions to modify the inventory accounting document ||  
|| `0` | Contractors should be provided by CRM | The CRM module is not active as a vendor for contractors ||  
|#  

{% include [System Errors](../../../_includes/system-errors.md) %}  

## Continue Learning  

- [{#T}](./catalog-documentcontractor-list.md)  
- [{#T}](./catalog-documentcontractor-add.md)  
- [{#T}](./catalog-documentcontractor-get-fields.md)  
