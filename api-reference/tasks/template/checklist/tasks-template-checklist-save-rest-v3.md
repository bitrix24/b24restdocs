# Save Entire Task Template Checklist v 3.0 tasks.template.checklist.save

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

The method `tasks.template.checklist.save` replaces a task template checklist with the passed set of items.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Task template identifier.

The identifier can be obtained with the [tasks.template.add](../tasks-template-add.md) method when creating a template or with the [tasks.template.list](../tasks-template-list.md) method ||
|| **items***
[`object[]`](../../../data-types.md) | An array of checklist items that must remain in the template after the call [(detailed description)](#items).

An empty array `[]` deletes all checklist items ||
|#

### Parameter items {#items}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of an existing checklist item of this template. An item with `id` is kept, and an item without `id` is created.

Item identifiers can be obtained with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method ||
|| **title**
[`string`](../../../data-types.md) | Item text. For a root item, the checklist title.

Required for a new item, that is, an item without `id` ||
|| **parentId**
[`integer`](../../../data-types.md) | Parent item identifier. The value `0` or `null` means the checklist root.

Only an item already saved in the template can be a parent. You cannot specify the item itself or an item nested in it.

If the field is not passed for an item with `id`, the parent does not change ||
|| **sortIndex**
[`integer`](../../../data-types.md) | Position of the item among the items of the same parent, zero-based. The value is `0` or greater.

If the field is not passed, a new item is placed according to its order in the request ||
|| **isComplete**
[`boolean`](../../../data-types.md) | Item completion flag.

Possible values:

- `true` — the item is completed
- `false` — the item is not completed

Default for a new item: `false` ||
|| **isImportant**
[`boolean`](../../../data-types.md) | Important item flag.

Possible values:

- `true` — the item is important
- `false` — a regular item

Default for a new item: `false` ||
|| **accomplices**
[`object[]`](../../../data-types.md) | Item accomplices: an array of objects with the `id` key, for example `[{"id":1269}]`.

User identifiers can be obtained with the [user.get](../../../user/user-get.md) method ||
|| **auditors**
[`object[]`](../../../data-types.md) | Item auditors: an array of objects with the `id` key, for example `[{"id":1}]`.

User identifiers can be obtained with the [user.get](../../../user/user-get.md) method ||
|#

How the method processes items:

- an item with `id` is kept, and fields missing from the request retain their current values
- an item without `id` is created
- template items missing from `items` are deleted
- attachments of kept items do not change: only the `tasks.template.checklist.item.attachment.*` methods change attachments

A new item can only be nested in an item that already exists in the template. A new root item and its subitems cannot be created in a single call: first save the root item, get its `id` with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method, and pass it in the `parentId` of the subitems in the next call. You can also add an item with the [tasks.template.checklist.item.add](./tasks-template-checklist-item-add-rest-v3.md) method, which returns the `id` immediately.

Accomplices and auditors of the items are added to the template accomplices and auditors.

The method supports the `Idempotency-Key` header: repeating a request with the same key and body does not perform the save a second time. [More about repeated calls without duplicates](../../../rest-v3.md#idempotency)

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.checklist.save`

{% endnote %}

In the example, item `159` is saved with new text, two new subitems are added to it, and the other template items are deleted.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":129,"items":[{"id":159,"title":"Stage 2"},{"title":"Step 1","parentId":159},{"title":"Step 2","parentId":159,"isImportant":true,"accomplices":[{"id":1269}]}]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.save
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":129,"items":[{"id":159,"title":"Stage 2"},{"title":"Step 1","parentId":159},{"title":"Step 2","parentId":159,"isImportant":true,"accomplices":[{"id":1269}]}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.checklist.save
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
    type TemplateCheckListSaveResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateCheckListSaveResult>(
        'tasks.template.checklist.save',
        {
          templateId: 129,
          items: [
            { id: 159, title: 'Stage 2' },
            { title: 'Step 1', parentId: 159 },
            { title: 'Step 2', parentId: 159, isImportant: true, accomplices: [{ id: 1269 }] },
          ],
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Checklist saved:', result.result)
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
      async function saveTemplateCheckList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.checklist.save',
            {
              templateId: 129,
              items: [
                { id: 159, title: 'Stage 2' },
                { title: 'Step 1', parentId: 159 },
                { title: 'Step 2', parentId: 159, isImportant: true, accomplices: [{ id: 1269 }] },
              ],
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Checklist saved:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', saveTemplateCheckList)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.checklist.save"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 129,
                "items": [
                    {"id": 159, "title": "Stage 2"},
                    {"title": "Step 1", "parentId": 159},
                    {"title": "Step 2", "parentId": 159, "isImportant": True, "accomplices": [{"id": 1269}]},
                ],
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
                'tasks.template.checklist.save',
                [
                    'templateId' => 129,
                    'items' => [
                        ['id' => 159, 'title' => 'Stage 2'],
                        ['title' => 'Step 1', 'parentId' => 159],
                        ['title' => 'Step 2', 'parentId' => 159, 'isImportant' => true, 'accomplices' => [['id' => 1269]]],
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
        'tasks.template.checklist.save',
        {
            templateId: 129,
            items: [
                { id: 159, title: 'Stage 2' },
                { title: 'Step 1', parentId: 159 },
                { title: 'Step 2', parentId: 159, isImportant: true, accomplices: [{ id: 1269 }] },
            ],
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
        'tasks.template.checklist.save',
        [
            'templateId' => 129,
            'items' => [
                ['id' => 159, 'title' => 'Stage 2'],
                ['title' => 'Step 1', 'parentId' => 159],
                ['title' => 'Step 2', 'parentId' => 159, 'isImportant' => true, 'accomplices' => [['id' => 1269]]],
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
    res, err := client.Core().Call(ctx, "tasks.template.checklist.save", b24.Params{
    	"templateId": 129,
    	"items": []map[string]any{
    		{"id": 159, "title": "Stage 2"},
    		{"title": "Step 1", "parentId": 159},
    		{"title": "Step 2", "parentId": 159, "isImportant": true, "accomplices": []map[string]any{{"id": 1269}}},
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.checklist.save: %w", err)
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

Contains an object with the `result` key and the `true` value if the checklist is saved. The saved items and their identifiers can be retrieved with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method ||
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
                "message": "Required field \"items.0.title\" is missing.",
                "field": "items.0.title"
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
|| `items` | Required field `items` is missing | Add `items` to the request body. To clear the checklist, pass an empty array ||
|| `items.0` | This request requires data type `array` in field `items.0` | Pass each item as an object, for example `{"title":"Step 1"}`. The number in the field name is the zero-based position of the item in the array ||
|| `items.0.title` | Required field `items.0.title` is missing | Pass `title` for a new item or `id` for an existing one ||
|#

#### Item Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

HTTP Status: **400**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `creatorId` | Field `creatorId` is read-only | Remove the read-only fields from the item: `creatorId`, `toggledById`, `toggledDate`, `attachments`, `collapsed` ||
|| `sortIndex` | Field value must be 0 or greater | Pass `sortIndex` of `0` or greater ||
|#

#### Unknown Item Field

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP Status: **400**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `CheckListItemDto` | Pass only the fields from the [items parameter description](#items) in the item ||
|#

#### Item Cannot Be Nested in Itself

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_CHECKLIST_ITEMCYCLEEXCEPTION`

HTTP Status: **400**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Cannot nest item with ID `159` under item with ID `159`: it is the same item or one of its nested items | In `parentId`, specify an item that is neither the item itself nor nested in it ||
|#

#### Template or Item Not Found

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

HTTP Status: **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Record with ID `999999` was not found | The message contains the identifier of the template, the item from `id`, or the item from `parentId`. Check the template with the [tasks.template.list](../tasks-template-list.md) method and the items with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method. The method does not find an item of another template either ||
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

- [{#T}](./tasks-template-checklist-get-rest-v3.md)
- [{#T}](./tasks-template-checklist-item-add-rest-v3.md)
- [{#T}](./tasks-template-checklist-item-update-rest-v3.md)
- [{#T}](./tasks-template-checklist-item-delete-rest-v3.md)
