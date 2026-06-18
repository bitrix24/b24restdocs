# Get Connector Fields biconnector.connector.fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
> 
> Who can execute the method: any user

The method `biconnector.connector.fields` returns a description of the connector fields. A table with the description of standard fields can be found in the article [Connector: Overview of Methods](./index.md#fields).

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.connector.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/biconnector.connector.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ConnectorFieldsResult = {
      fields: {
        title: string
        type: string
        isRequired: boolean
        isReadOnly: boolean
        isImmutable: boolean
        isMultiple: boolean
      }[]
    }

    try {
      const response = await $b24.actions.v2.call.make<ConnectorFieldsResult>({
        method: 'biconnector.connector.fields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.fields)
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
      async function getConnectorFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.connector.fields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.fields)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getConnectorFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'biconnector.connector.fields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling biconnector.connector.fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.connector.fields',
        {},
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.connector.fields',
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
    "fields": [
      {
        "title": "id",
        "type": "integer",
        "isRequired": true,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "title",
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "logo",
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "description",
        "type": "string",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "sort",
        "type": "integer",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "urlCheck",
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "urlData",
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "urlTableList",
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "urlTableDescription",
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "settings",
        "type": "array",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": true
      },
      {
        "title": "dateCreate",
        "type": "datetime",
        "isRequired": true,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false
      }
    ]
  },
  "time": {
    "start": 1740671757.058651,
    "finish": 1740671757.179896,
    "duration": 0.12124514579772949,
    "processing": 5.507469177246094e-5,
    "date_start": "2025-02-27T15:55:57+00:00",
    "date_finish": "2025-02-27T15:55:57+00:00"
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object in the format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...
    field_n: value_n,
}
```

where:
- `field_n` — connector field
- `value_n` — [field information](./index.md#description) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-connector-update.md)
- [{#T}](./biconnector-connector-get.md)
- [{#T}](./biconnector-connector-list.md)
- [{#T}](./biconnector-connector-delete.md)
- [{#T}](./biconnector-connector-add.md)