# Delete User Field userfieldconfig.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`userfieldconfig`](../../../scopes/permissions.md), module scope from `moduleId` (for example, [`crm`](../../../scopes/permissions.md))
>
> Who can execute the method: a user with permission to modify object settings in the `moduleId` module (for `crm` — permission "Allow to modify settings")

The `userfieldconfig.delete` method removes a user field.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../../../data-types.md) | Identifier of the module from which the field is being deleted ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the user field settings.

The identifier can be obtained using the [userfieldconfig.list](./userfieldconfig-list.md) method or when creating the field with the [userfieldconfig.add](./userfieldconfig-add.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","id":6953}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldconfig.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","id":6953,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/userfieldconfig.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<null>({
        method: 'userfieldconfig.delete',
        params: {
          moduleId: 'crm',
          id: 6953,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field deleted successfully')
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
      async function deleteUserField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'userfieldconfig.delete',
            params: {
              moduleId: 'crm',
              id: 6953,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field deleted successfully')
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteUserField)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'userfieldconfig.delete',
                [
                    'moduleId' => 'crm',
                    'id' => 6953,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Result: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'userfieldconfig.delete',
        {
            moduleId: 'crm',
            id: 6953,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldconfig.delete',
        [
            'moduleId' => 'crm',
            'id' => 6953,
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
    "result": null,
    "time": {
        "start": 1724239307.903115,
        "finish": 1724239308.567422,
        "duration": 0.6643068790435791,
        "processing": 0.20090818405151367,
        "date_start": "2024-08-21T13:21:47+02:00",
        "date_finish": "2024-08-21T13:21:48+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`null` | Returns `null` if the field is successfully deleted ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "You cannot delete the user field"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | You cannot delete the user field | Insufficient permissions to delete the field. This same error is returned if the field with the provided `id` has already been deleted or is unavailable in the context of `moduleId` ||
|| `-` | The current method required more scopes. (crm) | The application does not have the required scope for the module from `moduleId` ||
|| `-` | No settings for UserFieldAccess | Access to user fields is not configured for the provided `moduleId` ||
|| `-` | Error while attempting to modify user field settings | General error in deleting the field ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldconfig-add.md)
- [{#T}](./userfieldconfig-update.md)
- [{#T}](./userfieldconfig-get.md)
- [{#T}](./userfieldconfig-list.md)
- [{#T}](./userfieldconfig-get-types.md)