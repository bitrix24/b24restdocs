# Get a list of email fields mail.message.field.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `mail.message.field.list` method returns a list of available email fields.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of description fields that need to be returned in the response.

Available fields:

- `name` — field name
- `type` — data type
- `title` — header
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — mandatory groups
- `filterable` — filter availability flag
- `sortable` — sorting availability flag
- `editable` — editability flag
- `multiple` — multiple value flag
- `elementType` — item type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","filterable","sortable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","filterable","sortable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.field.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MailMessageFieldListResult = {
      items: {
        name: string
        type: string
        title: string
        filterable: boolean
        sortable: boolean
      }[]
    }

    try {
      const response = await $b24.actions.v3.call.make<MailMessageFieldListResult>({
        method: 'mail.message.field.list',
        params: {
          select: [
            'name',
            'type',
            'title',
            'filterable',
            'sortable',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field count:', result.items.length)
        console.info(result.items)
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
      async function getMailMessageFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'mail.message.field.list',
            params: {
              select: [
                'name',
                'type',
                'title',
                'filterable',
                'sortable',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field count:', result.items.length)
          console.info(result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMailMessageFields)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mail.message.field.list',
                [
                    'select' => [
                        'name',
                        'type',
                        'title',
                        'filterable',
                        'sortable'
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
        'mail.message.field.list',
        {
            select: [
                'name',
                'type',
                'title',
                'filterable',
                'sortable'
            ]
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
        'mail.message.field.list',
        [
            'select' => [
                'name',
                'type',
                'title',
                'filterable',
                'sortable'
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
                "name": "id",
                "type": "int",
                "title": "id",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "mailboxId",
                "type": "int",
                "title": "mailboxId",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "mailboxEmail",
                "type": "string",
                "title": "mailboxEmail",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "subject",
                "type": "string",
                "title": "subject",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "from",
                "type": "string",
                "title": "from",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "to",
                "type": "string",
                "title": "to",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "cc",
                "type": "string",
                "title": "cc",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "date",
                "type": "string",
                "title": "date",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "isSeen",
                "type": "bool",
                "title": "isSeen",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "hasAttachments",
                "type": "bool",
                "title": "hasAttachments",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "url",
                "type": "string",
                "title": "url",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "bindings",
                "type": "array",
                "title": "bindings",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "body",
                "type": "string",
                "title": "body",
                "filterable": false,
                "sortable": false
            }
        ]
    },
    "time": {
        "start": 1779819678,
        "finish": 1779819678.84803,
        "duration": 0.8480300903320312,
        "processing": 0,
        "date_start": "2026-05-26T11:21:18+03:00",
        "date_finish": "2026-05-26T11:21:18+03:00",
        "operating_reset_at": 1779820278,
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
[`array`](../../../data-types.md) | Array of objects with field descriptions. The response structure depends on `select` ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field `unknownField` for entity `DtoFieldDto`"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors
Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `-` | Access denied | Check user permissions and scope `mail` ||
|#

#### Errors in the `select` parameter

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `select` | Unknown field `#FIELD#` for entity `DtoFieldDto` | Pass only fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `multiple`, `elementType` ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-field-get.md)
- [{#T}](./index.md)
