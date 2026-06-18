# Delete localization sale.statusLang.deleteByFilter

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes the localization records of the order or delivery status by status ID and language.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`| **Description** ||
|| **fields***
[`object`](../../data-types.md) | Values of the filter fields (detailed description provided [below](#parametr-fields)) for deleting the localization record in the form of a structure:

```js
fields: {
    statusId: 'value',
    lid: 'value'
    name: 'value'
}
```

||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`| **Description** ||
|| **statusId***
[`sale_status.id`](../data-types.md) | Symbolic identifier of the status ||
|| **lid***
[`sale_lang.lid`](../data-types.md) | Language identifier. Current possible values are listed below:

- ar — Arabic
- br — Portuguese
- de — German
- en — English
- fr — French
- hi — Hindi
- id — Indonesian
- in — English (India)
- it — Italian
- ja — Japanese
- la — Spanish
- ms — Malay
- pl — Polish
- de — German
- sc — Chinese (simplified)
- tc — Chinese (traditional)
- th — Thai
- tr — Turkish
- ua — Ukrainian
- vn — Vietnamese

The list of available languages can be obtained using the method [sale.statuslang.getlistlangs](./sale-status-lang-get-list-langs.md) ||
|| **name***
[`string`](../../data-types.md) | 

{% note alert "Attention" %}

To ensure the method works correctly, please provide any non-empty value for this parameter 

{% endnote %}

||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"statusId":"RD","lid":"la","name":"-"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.statusLang.deleteByFilter
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"statusId":"RD","lid":"la","name":"-"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.statusLang.deleteByFilter
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
        method: 'sale.statusLang.deleteByFilter',
        params: {
          fields: {
            statusId: 'RD',
            lid: 'la',
            name: '-',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Status language deleted:', result)
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
      async function deleteStatusLang() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.statusLang.deleteByFilter',
            params: {
              fields: {
                statusId: 'RD',
                lid: 'la',
                name: '-',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Status language deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteStatusLang)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.statusLang.deleteByFilter',
                [
                    'fields' => [
                        'statusId' => 'RD',
                        'lid'     => 'la',
                        'name'    => '-',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting status language: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.statusLang.deleteByFilter',
        {
            fields: {
                statusId: 'RD',
                lid: 'la',
                name: '-',
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.statusLang.deleteByFilter',
        [
            'fields' =>
            [
                'statusId' => 'RD',
                'lid' => 'la',
                'name' => '-',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result":true,
    "time":{
        "start":1712223882.72792,
        "finish":1712223882.978806,
        "duration":0.2508859634399414,
        "processing":0.022897958755493164,
        "date_start":"2024-04-04T12:44:42+03:00",
        "date_finish":"2024-04-04T12:44:42+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the status localization deletion ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"Required fields: name"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201740400001` | Localization for deletion by the specified filter not found ||
|| `201750000004` | Unknown localization language identifier `lid` provided ||
|| `200040300010` | Insufficient permissions to delete the status localization ||
|| `100` | Parameter `fields` not specified or empty ||
|| `0` | Required fields of the `fields` structure not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-status-lang-get-list-langs.md)
- [{#T}](./sale-status-lang-add.md)
- [{#T}](./sale-status-lang-list.md)
- [{#T}](./sale-status-lang-get-fields.md)
