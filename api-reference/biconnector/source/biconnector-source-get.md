# Get Source by ID biconnector.source.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the "Analyst Workspace" section

The method `biconnector.source.get` returns information about the source by its identifier.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the source, which can be obtained using the methods [biconnector.source.list](./biconnector-source-list.md) and [biconnector.source.add](./biconnector-source-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.source.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/biconnector.source.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SourceGetResult = {
      item: {
        connection: {
          id: number
          type: string
          code: string
          title: string
          description: string
          active: boolean
          dateCreate: ISODate | null
          dateUpdate: ISODate | null
          createdById: number
          updatedById: number
        }
        connectorId: number
        settings: Array<{
          code: string
          name: string
          type: string
          value: string
          id: number
        }>
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<SourceGetResult>({
        method: 'biconnector.source.get',
        params: {
          id: 6,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.connection.id, result.item.connection.title, result.item.settings)
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
      async function getSource() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.source.get',
            params: {
              id: 6,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.connection.id, result.item.connection.title, result.item.settings)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSource)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'biconnector.source.get',
                [
                    'id' => 6,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting source: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.source.get',
        {
            id: 6,
        },
        (result) => {
            result.error() ? console.error(result.error()) : console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.source.get',
        [
            'id' => 6
        ]
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
        "item": {
            "connection": {
                "id": 6,
                "type": "rest",
                "code": "rest_1",
                "title": "Rest source SQL",
                "description": "Connection for working with mySql",
                "active": true,
                "dateCreate": "2025-03-20 14:50:06",
                "dateUpdate": "2025-03-20 14:50:06",
                "createdById": 1,
                "updatedById": 1
            },
            "connectorId": 1,
            "settings": [
                {
                    "code": "token",
                    "name": "Token",
                    "type": "STRING",
                    "value": "beliberda",
                    "id": 8
                }
            ]
        }
    },
    "time": {
        "start": 1742929480.368097,
        "finish": 1742929480.449558,
        "duration": 0.08146095275878906,
        "processing": 0.006555080413818359,
        "date_start": "2025-03-25T19:04:40+00:00",
        "date_finish": "2025-03-25T19:04:40+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`item`](../../data-types.md) | Root element of the response. Contains information about the source fields. Field descriptions can be found in the article [Sources: Overview of Methods](./index.md#fields) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **200**

```json
{
    "error": "VALIDATION_ID_NOT_PROVIDED",
    "error_description": "ID is missing."
}
```
{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `VALIDATION_ID_NOT_PROVIDED` | ID is missing. | Identifier is not provided ||
|| `VALIDATION_INVALID_ID_FORMAT` | ID has to be a positive integer. | Invalid ID format ||
|| `SOURCE_NOT_FOUND` | Source was not found. | Source not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-source-update.md)
- [{#T}](./biconnector-source-add.md)
- [{#T}](./biconnector-source-list.md)
- [{#T}](./biconnector-source-delete.md)
- [{#T}](./biconnector-source-fields.md)