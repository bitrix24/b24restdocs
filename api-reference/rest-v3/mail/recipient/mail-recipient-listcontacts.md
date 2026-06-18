# Get a list of contacts mail.recipient.listcontacts

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `mail.recipient.listcontacts` method searches for contacts in the current user's address book.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **query**
[`string`](../../../data-types.md) | Search string by contact name or email.

If not provided, the method returns the latest contacts ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters:
- `page` — page number
- `limit` — number of records per page, default is `50`, maximum `200`
- `offset` — record offset. If `page` and `limit` are provided, the offset is calculated automatically ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.recipient.listcontacts`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"query":"client","pagination":{"page":1,"limit":20,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.recipient.listcontacts
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"query":"client","pagination":{"page":1,"limit":20,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.recipient.listcontacts
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ListContactsResult = {
      items: {
        id: number
        email: string
        name: string
      }[]
    }

    try {
      const response = await $b24.actions.v3.call.make<ListContactsResult>({
        method: 'mail.recipient.listcontacts',
        params: {
          query: 'client',
          pagination: {
            page: 1,
            limit: 20,
            offset: 0,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Contacts:', result.items)
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
      async function listContacts() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'mail.recipient.listcontacts',
            params: {
              query: 'client',
              pagination: {
                page: 1,
                limit: 20,
                offset: 0,
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
          console.info('Contacts:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listContacts)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mail.recipient.listcontacts',
                [
                    'query' => 'client',
                    'pagination' => [
                        'page' => 1,
                        'limit' => 20,
                        'offset' => 0
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'mail.recipient.listcontacts',
        {
            query: 'client',
            pagination: {
                page: 1,
                limit: 20,
                offset: 0
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'mail.recipient.listcontacts',
        [
            'query' => 'client',
            'pagination' => [
                'page' => 1,
                'limit' => 20,
                'offset' => 0
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
    "result": {
        "items": [
            {
                "id": 10,
                "email": "client@example.com",
                "name": "Client"
            }
        ]
    },
    "time": {
        "start": 1779820087,
        "finish": 1779820087.962438,
        "duration": 0.9624381065368652,
        "processing": 0,
        "date_start": "2026-05-26T21:28:07+03:00",
        "date_finish": "2026-05-26T21:28:07+03:00",
        "operating_reset_at": 1779820687,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Response data object ||
|| **items**
[`array`](../../../data-types.md) | Array of contact objects ||
|| **items[]**
[`object`](../../../data-types.md) | Contact object ||
|| **id**
[`integer`](../../../data-types.md) | Contact identifier ||
|| **email**
[`string`](../../../data-types.md) | Contact email ||
|| **name**
[`string`](../../../data-types.md) | Contact name ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION",
        "message": "Unable to recognize pagination parameter `{\"limit\":\"abc\"}`"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | Check user permissions and `mail` scope ||
|#

#### Pagination Errors
Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `limit`
`offset`
`page` | Unable to recognize pagination parameter `#PAGE#` | Provide numeric values. `limit` must not be equal to `0` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-recipient-listemployees.md)
- [{#T}](../message/mail-message-send.md)
- [{#T}](./index.md)
