# Detach Files from Task Template Checklist Item v 3.0 tasks.template.checklist.item.attachment.detach

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

The method `tasks.template.checklist.item.attachment.detach` detaches attachments from a task template checklist item.

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
|| **attachmentIds***
[`integer[]`](../../../data-types.md) | An array of attachment identifiers. Pass attachment identifiers, not Drive file identifiers.

Attachment identifiers are returned by the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method in the `attachments[].id` field ||
|#

A file attached from Drive with the [tasks.template.checklist.item.attachment.attach](./tasks-template-checklist-item-attachment-attach-rest-v3.md) method remains on Drive after it is detached. A file uploaded with the [tasks.template.checklist.item.attachment.upload](./tasks-template-checklist-item-attachment-upload-rest-v3.md) method is deleted from Drive after it is detached.

If the item has no attachment from `attachmentIds`, the method returns the `Record with ID "..." was not found.` error with the item identifier, not the attachment identifier.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.checklist.item.attachment.detach`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":115,"itemId":153,"attachmentIds":[651]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.item.attachment.detach
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":115,"itemId":153,"attachmentIds":[651],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.checklist.item.attachment.detach
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
    type TemplateCheckListDetachResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateCheckListDetachResult>(
        'tasks.template.checklist.item.attachment.detach',
        {
          templateId: 115,
          itemId: 153,
          attachmentIds: [651],
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Files detached:', result.result)
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
      async function detachTemplateCheckListFiles() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.checklist.item.attachment.detach',
            {
              templateId: 115,
              itemId: 153,
              attachmentIds: [651],
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Files detached:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', detachTemplateCheckListFiles)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.checklist.item.attachment.detach"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 115,
                "itemId": 153,
                "attachmentIds": [651],
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
                'tasks.template.checklist.item.attachment.detach',
                [
                    'templateId' => 115,
                    'itemId' => 153,
                    'attachmentIds' => [651],
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
        'tasks.template.checklist.item.attachment.detach',
        {
            templateId: 115,
            itemId: 153,
            attachmentIds: [651],
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
        'tasks.template.checklist.item.attachment.detach',
        [
            'templateId' => 115,
            'itemId' => 153,
            'attachmentIds' => [651],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.checklist.item.attachment.detach", b24.Params{
    	"templateId":    115,
    	"itemId":        153,
    	"attachmentIds": []int{651},
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.checklist.item.attachment.detach: %w", err)
    }

    var item struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "result": true
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
[`boolean`](../../../data-types.md) | Root element of the response.

Contains an object with the `result` key and the `true` value if the attachments are detached. The current state of the checklist can be retrieved with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **404**

```json
{
    "error": {
        "code": "BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION",
        "message": "Record with ID \"153\" was not found."
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
|| `attachmentIds` | Required field `attachmentIds` is missing | Add `attachmentIds` to the request body ||
|| `attachmentIds` | This request requires data type `integer` in field `attachmentIds` | Pass the array elements as numbers, not strings ||
|#

#### Template, Item, or Attachment Not Found

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

HTTP Status: **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Record with ID `153` was not found | The message contains the template or item identifier. Check the template with the [tasks.template.list](../tasks-template-list.md) method and the item and its attachments with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method. The error with the item identifier also occurs when the item has no attachment from `attachmentIds`, for example, if a Drive file identifier is passed instead of an attachment identifier ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP Status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to modify the template and the new task card is available in Bitrix24 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-item-attachment-attach-rest-v3.md)
- [{#T}](./tasks-template-checklist-item-attachment-upload-rest-v3.md)
- [{#T}](./tasks-template-checklist-get-rest-v3.md)
- [{#T}](./tasks-template-checklist-remove-attachments.md)
