# Get Task Template Files v 3.0 tasks.template.file.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the template

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `tasks.template.file.list` method returns files attached to a task template.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../data-types.md) | Task template ID.

The ID can be retrieved with the [tasks.template.add](./tasks-template-add.md) method when creating a template or with the [tasks.template.list](./tasks-template-list.md) method ||
|| **ids**
[`integer[]`](../../data-types.md) | Array of template attachment IDs. Pass it to retrieve only these files.

The attachment ID is returned in the `id` field of this method's response. A Drive file ID does not work here: files with such values are not returned.

If the array is empty or the parameter is not passed, the method returns all template files ||
|#

The method skips attachment IDs that do not belong to the template without an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.file.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":113,"ids":[647,649]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.file.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":113,"ids":[647,649],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.file.list
    ```

- JS (TS)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TemplateFileListResult = {
      items: Array<{
        id: number
        objectId: number
        name: string
        type: string
        size: number
        width: number
        height: number
        isImage: boolean
        isVideo: boolean
        downloadUrl: string
        previewUrl: string | null
        ownerId: number
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateFileListResult>(
        'tasks.template.file.list',
        {
          templateId: 113,
          ids: [647, 649],
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.items)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listTemplateFiles() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.file.list',
            {
              templateId: 113,
              ids: [647, 649],
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTemplateFiles)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet included in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.file.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 113,
                "ids": [647, 649],
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.template.file.list',
                [
                    'templateId' => 113,
                    'ids' => [647, 649],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'tasks.template.file.list',
        {
            templateId: 113,
            ids: [647, 649],
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.template.file.list',
        [
            'templateId' => 113,
            'ids' => [647, 649],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.file.list", b24.Params{
    	"templateId": 113,
    	"ids":        []int{647, 649},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.file.list: %w", err)
    }

    // The method wraps the response in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("the response has no items key")
    }

    var files []struct {
    	ID       int    `json:"id"`
    	ObjectID int    `json:"objectId"`
    	Name     string `json:"name"`
    	Size     int    `json:"size"`
    }
    if err := json.Unmarshal(raw, &files); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(len(files))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 647,
                "objectId": 11573,
                "name": "reglament.docx",
                "type": "text/plain",
                "size": 13,
                "width": 0,
                "height": 0,
                "isImage": false,
                "isVideo": false,
                "downloadUrl": "/bitrix/services/main/ajax.php?action=disk.attachedObject.download&SITE_ID=s1&attachedObjectId=647",
                "previewUrl": null,
                "ownerId": 1269
            },
            {
                "id": 649,
                "objectId": 11575,
                "name": "price-list.xlsx",
                "type": "text/plain",
                "size": 13,
                "width": 0,
                "height": 0,
                "isImage": false,
                "isVideo": false,
                "downloadUrl": "/bitrix/services/main/ajax.php?action=disk.attachedObject.download&SITE_ID=s1&attachedObjectId=649",
                "previewUrl": null,
                "ownerId": 1269
            }
        ]
    },
    "time": {
        "start": 1795429006,
        "finish": 1795429006.852523,
        "duration": 0.8525230884552002,
        "processing": 0,
        "date_start": "2026-11-23T11:16:46+01:00",
        "date_finish": "2026-11-23T11:16:46+01:00",
        "operating_reset_at": 1795429560,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data ||
|| **items**
[`object[]`](../../data-types.md) | Array of template files [(detailed description)](#items). If the template has no files, the array is empty ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object items {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Template attachment ID. Use it to detach the file with the [tasks.template.file.detach](./tasks-template-file-detach-rest-v3.md) method ||
|| **objectId**
[`integer`](../../data-types.md) | Drive file ID. Use it to attach the file with the [tasks.template.file.attach](./tasks-template-file-attach-rest-v3.md) method ||
|| **name**
[`string`](../../data-types.md) | File name with extension ||
|| **type**
[`string`](../../data-types.md) | MIME type of the content, for example, `image/png` ||
|| **size**
[`integer`](../../data-types.md) | File size in bytes ||
|| **width**
[`integer`](../../data-types.md) | Image width in pixels. For files that are not images, `0` ||
|| **height**
[`integer`](../../data-types.md) | Image height in pixels. For files that are not images, `0` ||
|| **isImage**
[`boolean`](../../data-types.md) | Image flag.

Possible values:

- `true` — the file is an image
- `false` — the file is not an image ||
|| **isVideo**
[`boolean`](../../data-types.md) | Video flag.

Possible values:

- `true` — the file is a video
- `false` — the file is not a video ||
|| **downloadUrl**
[`string`](../../data-types.md) | Relative file download link. To download the file, prepend the Bitrix24 address to it ||
|| **previewUrl**
[`string`](../../data-types.md) | File preview link. If there is no preview, `null` is returned ||
|| **ownerId**
[`integer`](../../data-types.md) | ID of the user who uploaded the file ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "This request requires data type \"integer\" in field \"ids\".",
                "field": "ids"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `templateId` | Required field `templateId` is missing | Add `templateId` to the request body ||
|| `templateId` | This request requires data type `int` in field `templateId` | Pass the template ID as a number ||
|| `ids` | This request requires data type `array` in field `ids` | Pass `ids` as an array, for example, `[647]` ||
|| `ids` | This request requires data type `integer` in field `ids` | Pass the array elements as numbers, not strings ||
|#

#### Template Not Found

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Record with ID `999999` was not found | Check the template ID with the [tasks.template.list](./tasks-template-list.md) method. The method does not find a deleted template either ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to view the template and the new task card is available on Bitrix24 ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-file-attach-rest-v3.md)
- [{#T}](./tasks-template-file-detach-rest-v3.md)
- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
