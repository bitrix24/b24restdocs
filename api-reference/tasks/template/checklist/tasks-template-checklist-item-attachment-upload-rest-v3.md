# Upload File to Task Template Checklist Item v 3.0 tasks.template.checklist.item.attachment.upload

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify the template

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `tasks.template.checklist.item.attachment.upload` creates a file from the passed content and attaches it to a task template checklist item.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Task template identifier.

The identifier can be obtained with the [tasks.template.add](../tasks-template-add.md) method when creating a template or with the [tasks.template.list](../tasks-template-list.md) method ||
|| **itemId***
[`integer`](../../../data-types.md) | Checklist item identifier. The item must belong to the `templateId` template.

Item identifiers can be obtained with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method ||
|| **name***
[`string`](../../../data-types.md) | File name with extension, for example `notes.txt` ||
|| **content***
[`string`](../../../data-types.md) | File content encoded in Base64 ||
|#

The file is saved to the Drive of the user on whose behalf the request is made, in the "Uploaded files" folder. If a file with the same name already exists there, a number is added to the name, for example `notes (1).txt`.

Repeating a request without the `Idempotency-Key` header creates a second file. With the header, a repeat with the same key and body returns the previous response. [More about repeated calls without duplicates](../../../rest-v3.md#idempotency)

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.checklist.item.attachment.upload`

{% endnote %}

The cURL examples pass the `Idempotency-Key` header so that a repeated request does not create a second file.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -H "Idempotency-Key: 9f1c1a7e-5f1b-4a1e-9a2c-2f5f2b6c7d80" \
    -d '{"templateId":115,"itemId":153,"name":"notes.txt","content":"VGVzdCBmaWxlIGZvciBjaGVja2xpc3QgZG9jcw=="}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.item.attachment.upload
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -H "Idempotency-Key: 9f1c1a7e-5f1b-4a1e-9a2c-2f5f2b6c7d80" \
    -d '{"templateId":115,"itemId":153,"name":"notes.txt","content":"VGVzdCBmaWxlIGZvciBjaGVja2xpc3QgZG9jcw==","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.checklist.item.attachment.upload
    ```

- JS (TS)

    REST 3.0 task template methods are not yet in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client, `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TemplateCheckListUploadResult = {
      item: {
        id: number
        objectId: number
        name: string
        size: number
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateCheckListUploadResult>(
        'tasks.template.checklist.item.attachment.upload',
        {
          templateId: 115,
          itemId: 153,
          name: 'notes.txt',
          content: 'VGVzdCBmaWxlIGZvciBjaGVja2xpc3QgZG9jcw==',
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Attachment created:', result.item.id, result.item.objectId)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    REST 3.0 task template methods are not yet in the list of methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client, `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function uploadTemplateCheckListFile() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.checklist.item.attachment.upload',
            {
              templateId: 115,
              itemId: 153,
              name: 'notes.txt',
              content: 'VGVzdCBmaWxlIGZvciBjaGVja2xpc3QgZG9jcw==',
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Attachment created:', result.item.id, result.item.objectId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', uploadTemplateCheckListFile)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.checklist.item.attachment.upload"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 115,
                "itemId": 153,
                "name": "notes.txt",
                "content": "VGVzdCBmaWxlIGZvciBjaGVja2xpc3QgZG9jcw==",
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.template.checklist.item.attachment.upload',
                [
                    'templateId' => 115,
                    'itemId' => 153,
                    'name' => 'notes.txt',
                    'content' => 'VGVzdCBmaWxlIGZvciBjaGVja2xpc3QgZG9jcw==',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'tasks.template.checklist.item.attachment.upload',
        {
            templateId: 115,
            itemId: 153,
            name: 'notes.txt',
            content: 'VGVzdCBmaWxlIGZvciBjaGVja2xpc3QgZG9jcw==',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.template.checklist.item.attachment.upload',
        [
            'templateId' => 115,
            'itemId' => 153,
            'name' => 'notes.txt',
            'content' => 'VGVzdCBmaWxlIGZvciBjaGVja2xpc3QgZG9jcw==',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.checklist.item.attachment.upload", b24.Params{
    	"templateId": 115,
    	"itemId":     153,
    	"name":       "notes.txt",
    	"content":    "VGVzdCBmaWxlIGZvciBjaGVja2xpc3QgZG9jcw==",
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.checklist.item.attachment.upload: %w", err)
    }

    // The method wraps the attachment in an object under the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID       int    `json:"id"`
    	ObjectID int    `json:"objectId"`
    	Name     string `json:"name"`
    	Size     int    `json:"size"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.ObjectID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "item": {
            "id": 653,
            "objectId": 11579,
            "name": "notes.txt",
            "type": "text/plain",
            "size": 28,
            "width": 0,
            "height": 0,
            "isImage": false,
            "isVideo": false,
            "downloadUrl": "/bitrix/services/main/ajax.php?action=disk.attachedObject.download&SITE_ID=s1&attachedObjectId=653",
            "previewUrl": null,
            "ownerId": 1269
        }
    },
    "time": {
        "start": 1795428995,
        "finish": 1795428995.564208,
        "duration": 0.5642080307006836,
        "processing": 0,
        "date_start": "2026-11-23T11:16:35+01:00",
        "date_finish": "2026-11-23T11:16:35+01:00",
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
[`object`](../../../data-types.md) | Object with response data ||
|| **item**
[`object`](../../../data-types.md) | The created attachment. [Attachment field description](./tasks-template-checklist-get-rest-v3.md#attachment) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Field value must not be empty.",
                "field": "name"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP Status: **400**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `templateId` | Required field `templateId` is missing | Add `templateId` to the request body ||
|| `templateId` | This request requires data type `int` in field `templateId` | Pass the template identifier as a number ||
|| `itemId` | Required field `itemId` is missing | Add `itemId` to the request body ||
|| `name` | Required field `name` is missing | Add `name` to the request body ||
|| `name` | Field value must not be empty | Pass the file name with an extension ||
|| `content` | Required field `content` is missing | Add `content` to the request body ||
|| `content` | Field value must not be empty | Pass the file content encoded in Base64 ||
|#

#### Template or Item Not Found

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

HTTP Status: **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Record with ID `999999` was not found | The message contains the template or item identifier. Check the template with the [tasks.template.list](../tasks-template-list.md) method and the item with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method. The method does not find an item of another template either ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP Status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to modify the template and the new task card is available in Bitrix24 ||
|#

#### Execution Error

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_OPERATIONFAILEDEXCEPTION`

HTTP Status: **500**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Could not complete the request. Try again. If the problem persists, please contact your Bitrix24 administrator | Repeat the request. If the error persists, contact your Bitrix24 administrator ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-item-attachment-attach-rest-v3.md)
- [{#T}](./tasks-template-checklist-item-attachment-detach-rest-v3.md)
- [{#T}](./tasks-template-checklist-get-rest-v3.md)
- [{#T}](./tasks-template-checklist-add-attachment-by-content.md)
