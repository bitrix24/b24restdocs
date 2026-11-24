# Update Task Template Checklist Item v 3.0 tasks.template.checklist.item.update

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

The method `tasks.template.checklist.item.update` updates a task template checklist item.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Task template identifier.

The identifier can be obtained with the [tasks.template.add](../tasks-template-add.md) method when creating a template or with the [tasks.template.list](../tasks-template-list.md) method ||
|| **id***
[`integer`](../../../data-types.md) | Checklist item identifier. The item must belong to the `templateId` template.

Item identifiers can be obtained with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | New item field values [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../../data-types.md) | New item text. Do not pass an empty string ||
|| **parentId**
[`integer`](../../../data-types.md) | Identifier of the new parent item of the same template. The value `0` or `null` means the checklist root.

You cannot specify the item itself or an item nested in it ||
|| **sortIndex**
[`integer`](../../../data-types.md) | New position of the item among the items of the same parent, zero-based. The value is `0` or greater ||
|| **isComplete**
[`boolean`](../../../data-types.md) | Item completion flag.

Possible values:

- `true` — the item is completed
- `false` — the item is not completed ||
|| **isImportant**
[`boolean`](../../../data-types.md) | Important item flag.

Possible values:

- `true` — the item is important
- `false` — a regular item ||
|| **accomplices**
[`object[]`](../../../data-types.md) | New list of item accomplices: an array of objects with the `id` key, for example `[{"id":1269}]`. Replaces the current list; an empty array removes all item accomplices.

User identifiers can be obtained with the [user.get](../../../user/user-get.md) method ||
|| **auditors**
[`object[]`](../../../data-types.md) | New list of item auditors: an array of objects with the `id` key, for example `[{"id":1}]`. Replaces the current list; an empty array removes all item auditors.

User identifiers can be obtained with the [user.get](../../../user/user-get.md) method ||
|#

The method changes only the passed fields; other fields retain their current values. When the position or parent changes, neighboring items are renumbered without gaps.

Item accomplices and auditors are added to the template accomplices and auditors.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.checklist.item.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":115,"id":155,"fields":{"title":"Double-check data","isImportant":true}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.item.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":115,"id":155,"fields":{"title":"Double-check data","isImportant":true},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.checklist.item.update
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
    type TemplateCheckListItemUpdateResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateCheckListItemUpdateResult>(
        'tasks.template.checklist.item.update',
        {
          templateId: 115,
          id: 155,
          fields: {
            title: 'Double-check data',
            isImportant: true,
          },
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Item updated:', result.result)
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
      async function updateTemplateCheckListItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.checklist.item.update',
            {
              templateId: 115,
              id: 155,
              fields: {
                title: 'Double-check data',
                isImportant: true,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Item updated:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateTemplateCheckListItem)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.checklist.item.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 115,
                "id": 155,
                "fields": {
                    "title": "Double-check data",
                    "isImportant": True,
                },
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
                'tasks.template.checklist.item.update',
                [
                    'templateId' => 115,
                    'id' => 155,
                    'fields' => [
                        'title' => 'Double-check data',
                        'isImportant' => true,
                    ],
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
        'tasks.template.checklist.item.update',
        {
            templateId: 115,
            id: 155,
            fields: {
                title: 'Double-check data',
                isImportant: true,
            },
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
        'tasks.template.checklist.item.update',
        [
            'templateId' => 115,
            'id' => 155,
            'fields' => [
                'title' => 'Double-check data',
                'isImportant' => true,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.checklist.item.update", b24.Params{
    	"templateId": 115,
    	"id":         155,
    	"fields": map[string]any{
    		"title":       "Double-check data",
    		"isImportant": true,
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.checklist.item.update: %w", err)
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

Contains an object with the `result` key and the `true` value if the item is updated. The current state of the checklist can be retrieved with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_CHECKLIST_ITEMCYCLEEXCEPTION",
        "message": "Cannot nest item with ID \"149\" under item with ID \"155\": it is the same item or one of its nested items."
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
|| `id` | Required field `id` is missing | Add `id` to the request body ||
|| `id` | This request requires data type `integer` in field `id` | Pass the item identifier as a positive number ||
|| `fields` | Required field `fields` is missing | Add `fields` to the request body ||
|#

#### Field Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

HTTP Status: **400**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `collapsed` | Field `collapsed` is read-only | Remove the read-only fields from `fields`: `id`, `creatorId`, `toggledById`, `toggledDate`, `attachments`, `collapsed` ||
|| `sortIndex` | Field value must be 0 or greater | Pass `sortIndex` of `0` or greater ||
|#

#### Unknown Field

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP Status: **400**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `CheckListItemDto` | Pass only the fields from the [fields parameter description](#fields) in `fields` ||
|#

#### Item Cannot Be Nested in Itself

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_CHECKLIST_ITEMCYCLEEXCEPTION`

HTTP Status: **400**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Cannot nest item with ID `149` under item with ID `155`: it is the same item or one of its nested items | In `parentId`, specify an item that is neither the item itself nor nested in it ||
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

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-item-add-rest-v3.md)
- [{#T}](./tasks-template-checklist-get-rest-v3.md)
- [{#T}](./tasks-template-checklist-item-delete-rest-v3.md)
- [{#T}](./tasks-template-checklist-item-move-rest-v3.md)
- [{#T}](./tasks-template-checklist-update.md)
