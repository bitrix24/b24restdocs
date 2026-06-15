# Get Application Storage Description disk.storage.getforapp

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `disk.storage.getforapp` returns the description of the storage associated with the application. If the storage does not exist, it creates one.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md)

{% endnote %}

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.storage.getforapp
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type StorageResult = {
      ID: string
      NAME: string
      CODE: string | null
      MODULE_ID: string
      ENTITY_TYPE: string
      ENTITY_ID: string
      ROOT_OBJECT_ID: string
    }

    try {
      const response = await $b24.actions.v2.call.make<StorageResult>({
        method: 'disk.storage.getforapp',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Storage ID:', result.ID, 'Name:', result.NAME)
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
      async function getStorageForApp() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'disk.storage.getforapp',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Storage ID:', result.ID, 'Name:', result.NAME)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getStorageForApp)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.storage.getforapp',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching storage: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.storage.getforapp",
        {},
        function (result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'disk.storage.getforapp',
        []
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
        "ID": "1366",
        "NAME": "bitrix.restapi",
        "CODE": null,
        "MODULE_ID": "disk",
        "ENTITY_TYPE": "restapp",
        "ENTITY_ID": "3",
        "ROOT_OBJECT_ID": "8910"
    },
    "time": {
        "start": 1770028927,
        "finish": 1770028927.990697,
        "duration": 0.990696907043457,
        "processing": 0,
        "date_start": "2026-02-02T10:42:07+01:00",
        "date_finish": "2026-02-02T10:42:07+01:00",
        "operating_reset_at": 1770029527,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array with the description of the storage fields ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the storage ||
|| **NAME**
[`string`](../../data-types.md) | Name of the storage ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the storage ||
|| **MODULE_ID**
[`string`](../../data-types.md) | Identifier of the module to which the storage belongs ||
|| **ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the object associated with the storage ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Identifier of the object associated with the storage ||
|| **ROOT_OBJECT_ID**
[`integer`](../../data-types.md) | Identifier of the root folder of the storage ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ACCESS_DENIED",
    "error_description":"Access denied! Application context required"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %} 

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | Access denied! Application context required | Method called outside the application context ||
|| `ERROR_NOT_FOUND` | Could not find application by app_id | Application not found by identifier ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-storage-add-folder.md)
- [{#T}](./disk-storage-get-children.md)
- [{#T}](./disk-storage-get-fields.md)
- [{#T}](./disk-storage-get-list.md)
- [{#T}](./disk-storage-get-types.md)
- [{#T}](./disk-storage-get.md)
- [{#T}](./disk-storage-rename.md)
- [{#T}](./disk-storage-upload-file.md)