# Get Task Template Checklist v 3.0 tasks.template.checklist.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the template

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `tasks.template.checklist.get` returns all items of a task template checklist as a single list.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Task template identifier.

The identifier can be obtained with the [tasks.template.add](../tasks-template-add.md) method when creating a template or with the [tasks.template.list](../tasks-template-list.md) method ||
|| **select**
[`array`](../../../data-types.md) | An array of item fields that the method returns. If `select` is not specified, the method returns all fields.

Available fields: `id`, `parentId`, `sortIndex`, `title`, `isComplete`, `isImportant`, `creatorId`, `toggledById`, `toggledDate`, `accomplices`, `auditors`, `attachments`, `collapsed`. [Item field description](#checklist-item)

For the `accomplices`, `auditors`, and `attachments` fields, you can request individual fields of the nested object using a dot, for example `["id","title","accomplices.name","attachments.objectId"]` ||
|#

The method reads the entire checklist without pagination. Item nesting is defined by the `parentId` field, and the order within a parent by the `sortIndex` field. Items in the response are not sorted by tree, so build the tree by `parentId`.

An item with `parentId` = `null` is at the root. A root item is the title of a separate checklist, and its subitems are the items of that checklist. For a template without a checklist, the method returns an empty `items` array.

{% note warning "" %}

The previous version method with the same name [tasks.template.checklist.get](./tasks-template-checklist-get.md) returns a single item by its identifier. In the previous version, the entire checklist is returned by the [tasks.template.checklist.list](./tasks-template-checklist-list.md) method.

{% endnote %}

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.checklist.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":115,"select":["id","parentId","sortIndex","title","isComplete","accomplices.name","attachments.objectId"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":115,"select":["id","parentId","sortIndex","title","isComplete","accomplices.name","attachments.objectId"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.checklist.get
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
    type TemplateCheckListGetResult = {
      items: Array<{
        id: number
        parentId: number | null
        sortIndex: number
        title: string
        isComplete: boolean
        accomplices: Array<{ name: string }>
        attachments: Array<{ objectId: number }>
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateCheckListGetResult>(
        'tasks.template.checklist.get',
        {
          templateId: 115,
          select: [
            'id',
            'parentId',
            'sortIndex',
            'title',
            'isComplete',
            'accomplices.name',
            'attachments.objectId',
          ],
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        // Items come as a flat list: build the tree by parentId
        for (const item of result.items) {
          console.info(item.id, item.parentId, item.title, item.isComplete)
        }
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
      async function getTemplateCheckList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.checklist.get',
            {
              templateId: 115,
              select: [
                'id',
                'parentId',
                'sortIndex',
                'title',
                'isComplete',
                'accomplices.name',
                'attachments.objectId',
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
          result.items.forEach(item => console.info(item.id, item.parentId, item.title, item.isComplete))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTemplateCheckList)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.checklist.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 115,
                "select": [
                    "id",
                    "parentId",
                    "sortIndex",
                    "title",
                    "isComplete",
                    "accomplices.name",
                    "attachments.objectId",
                ],
            },
            prefer_version=3,
        )
        result = response["result"]
        for item in result["items"]:
            print(item["id"], item["parentId"], item["title"], item["isComplete"])
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
                'tasks.template.checklist.get',
                [
                    'templateId' => 115,
                    'select' => [
                        'id',
                        'parentId',
                        'sortIndex',
                        'title',
                        'isComplete',
                        'accomplices.name',
                        'attachments.objectId',
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
        'tasks.template.checklist.get',
        {
            templateId: 115,
            select: [
                'id',
                'parentId',
                'sortIndex',
                'title',
                'isComplete',
                'accomplices.name',
                'attachments.objectId',
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
        'tasks.template.checklist.get',
        [
            'templateId' => 115,
            'select' => [
                'id',
                'parentId',
                'sortIndex',
                'title',
                'isComplete',
                'accomplices.name',
                'attachments.objectId',
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
    res, err := client.Core().Call(ctx, "tasks.template.checklist.get", b24.Params{
    	"templateId": 115,
    	"select": []string{
    		"id", "parentId", "sortIndex", "title", "isComplete",
    		"accomplices.name", "attachments.objectId",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.checklist.get: %w", err)
    }

    // The method wraps the items in an array under the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("no items key in the response")
    }

    var items []struct {
    	ID         int    `json:"id"`
    	ParentID   *int   `json:"parentId"`
    	SortIndex  int    `json:"sortIndex"`
    	Title      string `json:"title"`
    	IsComplete bool   `json:"isComplete"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range items {
    	fmt.Println(item.ID, item.ParentID, item.Title, item.IsComplete)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

Example response without the `select` parameter:

```json
{
    "result": {
        "items": [
            {
                "id": 155,
                "parentId": 149,
                "sortIndex": 0,
                "title": "Double-check data",
                "isComplete": false,
                "isImportant": true,
                "creatorId": null,
                "toggledById": null,
                "toggledDate": null,
                "accomplices": [],
                "auditors": [],
                "attachments": [
                    {
                        "id": 657,
                        "objectId": 11583,
                        "name": "report.txt",
                        "type": "text/plain",
                        "size": 28,
                        "width": 0,
                        "height": 0,
                        "isImage": false,
                        "isVideo": false,
                        "downloadUrl": "/bitrix/services/main/ajax.php?action=disk.attachedObject.download&SITE_ID=s1&attachedObjectId=657",
                        "previewUrl": null,
                        "ownerId": 1269
                    }
                ],
                "collapsed": false
            },
            {
                "id": 149,
                "parentId": null,
                "sortIndex": 0,
                "title": "Checklist",
                "isComplete": false,
                "isImportant": false,
                "creatorId": null,
                "toggledById": null,
                "toggledDate": null,
                "accomplices": [],
                "auditors": [],
                "attachments": [],
                "collapsed": false
            },
            {
                "id": 153,
                "parentId": 149,
                "sortIndex": 1,
                "title": "Collect data",
                "isComplete": true,
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
                "auditors": [],
                "attachments": [],
                "collapsed": false
            },
            {
                "id": 151,
                "parentId": null,
                "sortIndex": 1,
                "title": "Prepare report",
                "isComplete": false,
                "isImportant": false,
                "creatorId": null,
                "toggledById": null,
                "toggledDate": null,
                "accomplices": [],
                "auditors": [],
                "attachments": [],
                "collapsed": false
            }
        ]
    },
    "time": {
        "start": 1795429337,
        "finish": 1795429337.807546,
        "duration": 0.8075459003448486,
        "processing": 0,
        "date_start": "2026-11-23T11:22:17+01:00",
        "date_finish": "2026-11-23T11:22:17+01:00",
        "operating_reset_at": 1795429920,
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
|| **items**
[`object[]`](../../../data-types.md) | An array of template checklist items. Each element is a [checklist item object](#checklist-item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Checklist Item Object {#checklist-item}

The set of fields depends on the `select` parameter. Fields marked as read-only cannot be passed to the [tasks.template.checklist.item.add](./tasks-template-checklist-item-add-rest-v3.md), [tasks.template.checklist.item.update](./tasks-template-checklist-item-update-rest-v3.md), and [tasks.template.checklist.save](./tasks-template-checklist-save-rest-v3.md) methods.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Checklist item identifier ||
|| **parentId**
[`integer`](../../../data-types.md) | Identifier of the parent item of the same template. For a root item, `null` ||
|| **sortIndex**
[`integer`](../../../data-types.md) | Position of the item among the items of the same parent, zero-based ||
|| **title**
[`string`](../../../data-types.md) | Item text. For a root item, the checklist title ||
|| **isComplete**
[`boolean`](../../../data-types.md) | Item completion flag.

Possible values:

- `true` — the item is marked as completed
- `false` — the item is not completed ||
|| **isImportant**
[`boolean`](../../../data-types.md) | Important item flag.

Possible values:

- `true` — the item is marked as important
- `false` — a regular item ||
|| **creatorId**
[`integer`](../../../data-types.md) | Identifier of the user who created the item. Read-only.

In a template checklist, the value is `null` ||
|| **toggledById**
[`integer`](../../../data-types.md) | Identifier of the user who last marked the item as completed or cleared the mark. Read-only.

In a template checklist, the value is `null` ||
|| **toggledDate**
[`datetime`](../../../data-types.md) | Date and time of the last completion mark change. Read-only.

In a template checklist, the value is `null` ||
|| **accomplices**
[`object[]`](../../../data-types.md) | Item accomplices. Each element is a [user object](#user).

Item accomplices are added to the template accomplices ||
|| **auditors**
[`object[]`](../../../data-types.md) | Item auditors. Each element is a [user object](#user).

Item auditors are added to the template auditors ||
|| **attachments**
[`object[]`](../../../data-types.md) | Files attached to the item. Each element is an [attachment object](#attachment). Read-only.

Attachments are changed by the [tasks.template.checklist.item.attachment.attach](./tasks-template-checklist-item-attachment-attach-rest-v3.md), [tasks.template.checklist.item.attachment.upload](./tasks-template-checklist-item-attachment-upload-rest-v3.md), and [tasks.template.checklist.item.attachment.detach](./tasks-template-checklist-item-attachment-detach-rest-v3.md) methods ||
|| **collapsed**
[`boolean`](../../../data-types.md) | Collapsed item flag. Each user has their own value; the response contains the value of the user on whose behalf the request is made. Read-only.

Possible values:

- `true` — nested items are collapsed
- `false` — nested items are expanded

The value is changed by the [tasks.template.checklist.item.collapse](./tasks-template-checklist-item-collapse-rest-v3.md) and [tasks.template.checklist.item.expand](./tasks-template-checklist-item-expand-rest-v3.md) methods ||
|#

#### User Object {#user}

When writing to `accomplices` and `auditors`, it is enough to pass objects with the `id` key, for example `[{"id":1269}]`. User identifiers can be obtained with the [user.get](../../../user/user-get.md) method.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | User identifier ||
|| **name**
[`string`](../../../data-types.md) | User first and last name ||
|| **role**
[`string`](../../../data-types.md) | User role. In a template checklist, the value is `null` ||
|| **image**
[`object`](../../../data-types.md) | User avatar. Object keys: `id`, `src`, `name`, `width`, `height`, `size`, `subDir`, `contentType`, `file`. The image link is in the `src` key, the dimensions in pixels are in the `width` and `height` keys, and the size in bytes is in the `size` key ||
|| **gender**
[`string`](../../../data-types.md) | User gender.

Possible values:

- `M` — male
- `F` — female ||
|| **email**
[`string`](../../../data-types.md) | User email. In a template checklist, the value is `null` ||
|| **externalAuthId**
[`string`](../../../data-types.md) | User external authentication type. In a template checklist, the value is `null` ||
|| **rights**
[`array`](../../../data-types.md) | User permissions. In a template checklist, the value is an empty array ||
|#

#### Attachment Object {#attachment}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Attachment identifier. It is passed to the [tasks.template.checklist.item.attachment.detach](./tasks-template-checklist-item-attachment-detach-rest-v3.md) method ||
|| **objectId**
[`integer`](../../../data-types.md) | Drive file identifier. It is passed to the [tasks.template.checklist.item.attachment.attach](./tasks-template-checklist-item-attachment-attach-rest-v3.md) method and to the `disk.file.*` methods, for example [disk.file.get](../../../disk/file/disk-file-get.md) ||
|| **name**
[`string`](../../../data-types.md) | File name with extension ||
|| **type**
[`string`](../../../data-types.md) | File MIME type, for example `text/plain` ||
|| **size**
[`integer`](../../../data-types.md) | File size in bytes ||
|| **width**
[`integer`](../../../data-types.md) | Image width in pixels. For a file that is not an image, `0` ||
|| **height**
[`integer`](../../../data-types.md) | Image height in pixels. For a file that is not an image, `0` ||
|| **isImage**
[`boolean`](../../../data-types.md) | Image flag.

Possible values:

- `true` — the file is an image
- `false` — the file is not an image ||
|| **isVideo**
[`boolean`](../../../data-types.md) | Video flag.

Possible values:

- `true` — the file is a video
- `false` — the file is not a video ||
|| **downloadUrl**
[`string`](../../../data-types.md) | Relative file download link ||
|| **previewUrl**
[`string`](../../../data-types.md) | File preview link. If there is no preview, `null` ||
|| **ownerId**
[`integer`](../../../data-types.md) | Identifier of the user who uploaded the file ||
|#

## Error Handling

HTTP Status: **404**

```json
{
    "error": {
        "code": "BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION",
        "message": "Record with ID \"999999\" was not found."
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
|#

#### Unknown Field in select

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP Status: **400**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `CheckListItemDto` | In `select`, specify fields from the [checklist item description](#checklist-item) ||
|#

#### Template Not Found

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

HTTP Status: **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Record with ID `999999` was not found | Check the template identifier with the [tasks.template.list](../tasks-template-list.md) method. The method does not find a deleted template either ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP Status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to view the template and the new task card is available in Bitrix24 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-save-rest-v3.md)
- [{#T}](./tasks-template-checklist-item-add-rest-v3.md)
- [{#T}](./tasks-template-checklist-item-update-rest-v3.md)
- [{#T}](../tasks-template-get-rest-v3.md)
- [{#T}](./tasks-template-checklist-list.md)
