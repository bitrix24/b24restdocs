# Add Item to Task Template Checklist v 3.0 tasks.template.checklist.item.add

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

The method `tasks.template.checklist.item.add` adds a single item to a task template checklist.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Task template identifier.

The identifier can be obtained with the [tasks.template.add](../tasks-template-add.md) method when creating a template or with the [tasks.template.list](../tasks-template-list.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Field values of the new item [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../../data-types.md) | Item text. For a root item, the checklist title. An empty string is not allowed ||
|| **parentId**
[`integer`](../../../data-types.md) | Identifier of the parent item of the same template. The value `0` or `null` means the checklist root.

If the field is not passed, the item is added to the root. Item identifiers can be obtained with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method ||
|| **sortIndex**
[`integer`](../../../data-types.md) | Position of the item among the items of the same parent, zero-based. The value is `0` or greater.

If the field is not passed, the item is added last. If the position is taken, the following items are shifted ||
|| **isComplete**
[`boolean`](../../../data-types.md) | Item completion flag.

Possible values:

- `true` — the item is completed
- `false` — the item is not completed

Default: `false` ||
|| **isImportant**
[`boolean`](../../../data-types.md) | Important item flag.

Possible values:

- `true` — the item is important
- `false` — a regular item

Default: `false` ||
|| **accomplices**
[`object[]`](../../../data-types.md) | Item accomplices: an array of objects with the `id` key, for example `[{"id":1269}]`.

User identifiers can be obtained with the [user.get](../../../user/user-get.md) method ||
|| **auditors**
[`object[]`](../../../data-types.md) | Item auditors: an array of objects with the `id` key, for example `[{"id":1}]`.

User identifiers can be obtained with the [user.get](../../../user/user-get.md) method ||
|#

To create a new checklist, add a root item without `parentId`: its `title` becomes the checklist title. Then add items with the `parentId` of the root item.

Item accomplices and auditors are added to the template accomplices and auditors.

The method supports the `Idempotency-Key` header: repeating a request with the same key and body does not create a second item. [More about repeated calls without duplicates](../../../rest-v3.md#idempotency)

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.checklist.item.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":115,"fields":{"title":"Collect data","parentId":149,"isImportant":true,"accomplices":[{"id":1269}],"auditors":[{"id":1}]}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.item.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":115,"fields":{"title":"Collect data","parentId":149,"isImportant":true,"accomplices":[{"id":1269}],"auditors":[{"id":1}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.checklist.item.add
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
    type TemplateCheckListItemAddResult = {
      item: {
        id: number
        parentId: number | null
        sortIndex: number
        title: string
        isComplete: boolean
        isImportant: boolean
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateCheckListItemAddResult>(
        'tasks.template.checklist.item.add',
        {
          templateId: 115,
          fields: {
            title: 'Collect data',
            parentId: 149,
            isImportant: true,
            accomplices: [{ id: 1269 }],
            auditors: [{ id: 1 }],
          },
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Item created:', result.item.id, result.item.sortIndex)
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
      async function addTemplateCheckListItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.checklist.item.add',
            {
              templateId: 115,
              fields: {
                title: 'Collect data',
                parentId: 149,
                isImportant: true,
                accomplices: [{ id: 1269 }],
                auditors: [{ id: 1 }],
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
          console.info('Item created:', result.item.id, result.item.sortIndex)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTemplateCheckListItem)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.checklist.item.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 115,
                "fields": {
                    "title": "Collect data",
                    "parentId": 149,
                    "isImportant": True,
                    "accomplices": [{"id": 1269}],
                    "auditors": [{"id": 1}],
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
                'tasks.template.checklist.item.add',
                [
                    'templateId' => 115,
                    'fields' => [
                        'title' => 'Collect data',
                        'parentId' => 149,
                        'isImportant' => true,
                        'accomplices' => [['id' => 1269]],
                        'auditors' => [['id' => 1]],
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
        'tasks.template.checklist.item.add',
        {
            templateId: 115,
            fields: {
                title: 'Collect data',
                parentId: 149,
                isImportant: true,
                accomplices: [{ id: 1269 }],
                auditors: [{ id: 1 }],
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
        'tasks.template.checklist.item.add',
        [
            'templateId' => 115,
            'fields' => [
                'title' => 'Collect data',
                'parentId' => 149,
                'isImportant' => true,
                'accomplices' => [['id' => 1269]],
                'auditors' => [['id' => 1]],
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
    res, err := client.Core().Call(ctx, "tasks.template.checklist.item.add", b24.Params{
    	"templateId": 115,
    	"fields": map[string]any{
    		"title":       "Collect data",
    		"parentId":    149,
    		"isImportant": true,
    		"accomplices": []map[string]any{{"id": 1269}},
    		"auditors":    []map[string]any{{"id": 1}},
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.checklist.item.add: %w", err)
    }

    // The method wraps the item in an object under the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID        int    `json:"id"`
    	ParentID  *int   `json:"parentId"`
    	SortIndex int    `json:"sortIndex"`
    	Title     string `json:"title"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.SortIndex)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "item": {
            "id": 153,
            "parentId": 149,
            "sortIndex": 0,
            "title": "Collect data",
            "isComplete": false,
            "isImportant": true,
            "creatorId": null,
            "toggledById": null,
            "toggledDate": null,
            "accomplices": [
                {
                    "id": 1269,
                    "name": "Klaus Weber",
                    "role": null,
                    "image": {
                        "id": null,
                        "src": "https://cdn-com.bitrix24.com/b13743910/resize_cache/32741/45a41af90f4a8f8ae94152f60a6ec694/main/bd3/bd36722eacdbe1263e452943ce21a820/avatar.png",
                        "name": null,
                        "width": 100,
                        "height": 100,
                        "size": 24131,
                        "subDir": null,
                        "contentType": null,
                        "file": null
                    },
                    "gender": "M",
                    "email": null,
                    "externalAuthId": null,
                    "rights": []
                }
            ],
            "auditors": [
                {
                    "id": 1,
                    "name": "Anna Schmidt",
                    "role": null,
                    "image": {
                        "id": null,
                        "src": "https://cdn-com.bitrix24.com/b13743910/resize_cache/2267/45a41af90f4a8f8ae94152f60a6ec694/main/c7b/c7bd44b1babaa5448125dd97d038ce1b/avatar.jpg",
                        "name": null,
                        "width": 100,
                        "height": 100,
                        "size": 10001,
                        "subDir": null,
                        "contentType": null,
                        "file": null
                    },
                    "gender": "F",
                    "email": null,
                    "externalAuthId": null,
                    "rights": []
                }
            ],
            "attachments": [],
            "collapsed": false
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
[`object`](../../../data-types.md) | The created item as saved. [Checklist item field description](./tasks-template-checklist-get-rest-v3.md#checklist-item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Required field \"title\" is empty.",
                "field": "title"
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
|| `fields` | Required field `fields` is missing | Add `fields` to the request body ||
|| `-` | Property "title" must be set | Pass a non-empty string in `title` ||
|#

#### Field Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

HTTP Status: **400**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `title` | Required field `title` is empty | Add `title` to `fields` ||
|| `id` | Field `id` is read-only | Remove the read-only fields from `fields`: `id`, `creatorId`, `toggledById`, `toggledDate`, `attachments`, `collapsed` ||
|| `sortIndex` | Field value must be 0 or greater | Pass `sortIndex` of `0` or greater ||
|#

#### Unknown Field

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP Status: **400**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `CheckListItemDto` | Pass only the fields from the [fields parameter description](#fields) in `fields` ||
|#

#### Template or Item Not Found

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

HTTP Status: **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Record with ID `999999` was not found | The message contains the identifier of the template or of the item from `parentId`. Check the template with the [tasks.template.list](../tasks-template-list.md) method and the item with the [tasks.template.checklist.get](./tasks-template-checklist-get-rest-v3.md) method. The method does not find an item of another template either ||
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

- [{#T}](./tasks-template-checklist-item-update-rest-v3.md)
- [{#T}](./tasks-template-checklist-get-rest-v3.md)
- [{#T}](./tasks-template-checklist-item-delete-rest-v3.md)
- [{#T}](./tasks-template-checklist-save-rest-v3.md)
- [{#T}](./tasks-template-checklist-add.md)
