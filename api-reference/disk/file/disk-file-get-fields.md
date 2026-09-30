# Get Description of File Fields for disk.file.getFields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.file.getFields` returns the description of file fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/disk.file.getFields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/disk.file.getFields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldItem = {
      TYPE: string
      USE_IN_FILTER: boolean
      USE_IN_SHOW: boolean
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DiskFileFieldsResult = Record<string, FieldItem>

    try {
      const response = await $b24.actions.v2.call.make<DiskFileFieldsResult>({
        method: 'disk.file.getFields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('File fields:', Object.keys(result))
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
      async function getDiskFileFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'disk.file.getFields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('File fields:', Object.keys(result))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDiskFileFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.disk.file.getfields().response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'disk.file.getFields',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting file fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "disk.file.getFields",
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
        'disk.file.getFields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "disk.file.getFields", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("disk.file.getFields: %w", err)
    }

    keys, ok := b24.Keys(res.Result)
    if !ok {
    	return fmt.Errorf("expected an object in the response")
    }
    fmt.Println("fields in response:", len(keys))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": {
            "TYPE": "integer",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "NAME": {
            "TYPE": "string",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "TYPE": {
            "TYPE": "enum",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "CODE": {
            "TYPE": "string",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "STORAGE_ID": {
            "TYPE": "integer",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "PARENT_ID": {
            "TYPE": "integer",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "CREATE_TIME": {
            "TYPE": "datetime",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "UPDATE_TIME": {
            "TYPE": "datetime",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "DELETE_TIME": {
            "TYPE": "datetime",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        },
        "CREATED_BY": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "UPDATED_BY": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "DELETED_BY": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "GLOBAL_CONTENT_VERSION": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "FILE_ID": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "SIZE": {
            "TYPE": "integer",
            "USE_IN_FILTER": false,
            "USE_IN_SHOW": true
        },
        "DELETED_TYPE": {
            "TYPE": "enum",
            "USE_IN_FILTER": true,
            "USE_IN_SHOW": true
        }
    },
    "time": {
        "start": 1770651518,
        "finish": 1770651518.741429,
        "duration": 0.7414290904998779,
        "processing": 0,
        "date_start": "2026-02-09T16:38:38+01:00",
        "date_finish": "2026-02-09T16:38:38+01:00",
        "operating_reset_at": 1770652118,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing file field descriptions [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`object`](../../data-types.md) | Description of the "File identifier" field ||
|| **NAME**
[`object`](../../data-types.md) | Description of the "File name" field ||
|| **TYPE**
[`object`](../../data-types.md) | Description of the "Object type" field ||
|| **CODE**
[`object`](../../data-types.md) | Description of the "File symbolic code" field ||
|| **STORAGE_ID**
[`object`](../../data-types.md) | Description of the "Storage identifier" field ||
|| **PARENT_ID**
[`object`](../../data-types.md) | Description of the "Parent folder identifier" field ||
|| **CREATE_TIME**
[`object`](../../data-types.md) | Description of the "File creation date and time" field ||
|| **UPDATE_TIME**
[`object`](../../data-types.md) | Description of the "File last update date and time" field ||
|| **DELETE_TIME**
[`object`](../../data-types.md) | Description of the "Date and time the file was moved to the trash" field ||
|| **CREATED_BY**
[`object`](../../data-types.md) | Description of the "Identifier of the user who created the file" field ||
|| **UPDATED_BY**
[`object`](../../data-types.md) | Description of the "Identifier of the user who changed the file" field ||
|| **DELETED_BY**
[`object`](../../data-types.md) | Description of the "Identifier of the user who deleted the file" field ||
|| **GLOBAL_CONTENT_VERSION**
[`object`](../../data-types.md) | Description of the "Global content version" field ||
|| **FILE_ID**
[`object`](../../data-types.md) | Description of the "Internal file identifier" field ||
|| **SIZE**
[`object`](../../data-types.md) | Description of the "File size" field ||
|| **DELETED_TYPE**
[`object`](../../data-types.md) | Description of the "Object deletion status" field ||
|#

Each field in the `result` object contains a description with the following structure.

#### Field Description Structure

#|
|| **Name**
`type` | **Description** ||
|| **TYPE**
[`string`](../../data-types.md) | Field data type. Possible values: `integer`, `string`, `enum`, `datetime` ||
|| **USE_IN_FILTER**
[`boolean`](../../data-types.md) | Indicates whether the field is available for filtering ||
|| **USE_IN_SHOW**
[`boolean`](../../data-types.md) | Indicates whether the field is present in file data ||
|#

## Error Handling

This method has no method-specific error codes.

{% include notitle [error handling](../../../_includes/error-info.md) %}

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disk-file-copy-to.md)
- [{#T}](./disk-file-delete.md)
- [{#T}](./disk-file-get-external-link.md)
- [{#T}](./disk-file-get-versions.md)
- [{#T}](./disk-file-get.md)
- [{#T}](./disk-file-mark-deleted.md)
- [{#T}](./disk-file-move-to.md)
- [{#T}](./disk-file-rename.md)
- [{#T}](./disk-file-restore-from-version.md)
- [{#T}](./disk-file-restore.md)
- [{#T}](./disk-file-search.md)
- [{#T}](./disk-file-upload-version.md)
