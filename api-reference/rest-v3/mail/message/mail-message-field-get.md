# Get e-mail field mail.message.field.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`mail`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `mail.message.field.get` method returns a description of an e-mail field by name.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Name of the field whose description needs to be obtained.

Available fields:

- `id` — e-mail identifier
- `mailboxId` — mailbox identifier
- `mailboxEmail` — mailbox e-mail address
- `subject` — e-mail subject
- `from` — sender
- `to` — recipients
- `cc` — CC recipients
- `date` — e-mail date
- `isSeen` — read flag
- `hasAttachments` — attachments flag
- `url` — link to e-mail
- `bindings` — e-mail relations
- `body` — e-mail text ||
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
- `multiple` — multi-value flag
- `elementType` — item type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/mail.message.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"subject","select":["name","type","title"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/mail.message.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"subject","select":["name","type","title"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/mail.message.field.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FieldGetResult = {
      item: {
        name: string
        type: string
        title: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<FieldGetResult>({
        method: 'mail.message.field.get',
        params: {
          name: 'subject',
          select: [
            'name',
            'type',
            'title',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.name, result.item.type, result.item.title)
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
      async function getMessageField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'mail.message.field.get',
            params: {
              name: 'subject',
              select: [
                'name',
                'type',
                'title',
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
          console.info(result.item.name, result.item.type, result.item.title)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMessageField)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mail.message.field.get',
                [
                    'name' => 'subject',
                    'select' => [
                        'name',
                        'type',
                        'title'
                    ],
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
        'mail.message.field.get',
        {
            name: 'subject',
            select: [
                'name',
                'type',
                'title'
            ]
        },
        function(result) {
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
        'mail.message.field.get',
        [
            'name' => 'subject',
            'select' => [
                'name',
                'type',
                'title'
            ],
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
        "item": {
            "name": "subject",
            "type": "string",
            "title": "subject"
        }
    },
    "time": {
        "start": 1769780771,
        "finish": 1769780771.081992,
        "duration": 0.08199191093444824,
        "processing": 0,
        "date_start": "2026-05-25T16:46:11+03:00",
        "date_finish": "2026-05-25T16:46:11+03:00",
        "operating_reset_at": 1769781371,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **item**
[`object`](../../../data-types.md) | Object with field description. The response structure depends on `select` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "Mandatory field `name` is missing",
                "field": "name"
            }
        ]
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

#### Data Not Found Errors
Error code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `name` | Field `#FIELD#` not found | Specify an existing field name ||
|#

#### Request Validation Errors
Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `name` | Mandatory field `name` is not specified | Pass the parameter `name` with an existing field name ||
|#

#### Errors in the `select` Parameter

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `select` | Unknown field `#FIELD#` for entity `DtoFieldDto` | Pass only fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `multiple`, `elementType` ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as a string array, for example `["name","type"]` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mail-message-field-list.md)
- [{#T}](./index.md)
