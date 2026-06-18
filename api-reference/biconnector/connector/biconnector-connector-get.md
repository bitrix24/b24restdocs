# Get Connector by ID biconnector.connector.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the "Analyst Workspace" section

The method `biconnector.connector.get` returns information about the connector by its identifier.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | The identifier of the connector, which can be obtained using the methods [biconnector.connector.list](./biconnector-connector-list.md) and [biconnector.connector.add](./biconnector-connector-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "id": 4
             }' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.connector.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "id": 4,
             "auth": "**put_access_token_here**"
             }' \
         https://**put_your_bitrix24_address**/rest/biconnector.connector.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ConnectorGetResult = {
      item: {
        id: number
        title: string
        dateCreate: ISODate | null
        logo: string
        description: string
        sort: number
        urlCheck: string
        settings: Array<{
          name: string
          code: string
          type: string
        }>
        urlData: string
        urlTableList: string
        urlTableDescription: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ConnectorGetResult>({
        method: 'biconnector.connector.get',
        params: {
          id: 4,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.id, result.item.title, result.item.settings)
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
      async function getConnector() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.connector.get',
            params: {
              id: 4,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.id, result.item.title, result.item.settings)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getConnector)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'biconnector.connector.get',
                [
                    'id' => 4,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling biconnector.connector.get: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.connector.get',
        {
            id: 4,
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
        'biconnector.connector.get',
        [
            'id' => 4
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
            "id": 5,
            "title": "SUPER REST CONNECTOR",
            "dateCreate": "2025-03-24 07:25:59",
            "logo": "https://masterpiecer-images.s3.amazonaws.com/5fd531dca6427c7:upscaled",
            "description": "Connector with token",
            "sort": 100,
            "urlCheck": "http://app.domain/check",
            "settings": [
                {
                    "name": "Login",
                    "code": "login",
                    "type": "STRING"
                },
                {
                    "name": "Password",
                    "code": "password",
                    "type": "STRING"
                }
            ],
            "urlData": "http://app.domain/data",
            "urlTableList": "http://app.domain/table_list",
            "urlTableDescription": "http://app.domain/table_description"
        }
    },
    "time": {
        "start": 1725365418.056843,
        "finish": 1725365419.671506,
        "duration": 1.6146628856658936,
        "processing": 1.3475170135498047,
        "date_start": "2024-09-03T14:10:18+02:00",
        "date_finish": "2024-09-03T14:10:19+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`item`](../../data-types.md) | The root element of the response. Contains information about the connector fields. Field descriptions can be found in the article [Connector: Overview of Methods](./index.md#fields) ||
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
|| `CONNECTOR_NOT_FOUND` | Connector was not found. | Connector not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-connector-update.md)
- [{#T}](./biconnector-connector-add.md)
- [{#T}](./biconnector-connector-list.md)
- [{#T}](./biconnector-connector-delete.md)
- [{#T}](./biconnector-connector-fields.md)