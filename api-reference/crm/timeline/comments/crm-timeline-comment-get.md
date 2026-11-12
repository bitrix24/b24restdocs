# Get Information About the Comment crm.timeline.comment.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with read permission for at least one linked CRM item

The `crm.timeline.comment.get` method retrieves information about a comment in the CRM timeline.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Comment identifier, a positive integer. Identifiers can be obtained using the [`crm.timeline.comment.list`](./crm-timeline-comment-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.comment.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.comment.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TimelineCommentGetResult = {
      ID: string
      ENTITY_ID: string
      ENTITY_TYPE: string
      CREATED: ISODate
      COMMENT: string
      AUTHOR_ID: string
      FILES?: Record<string, {
        id: number
        date: ISODate
        type: string
        name: string
        size: number
        image: { width: number; height: number } | false
        authorId: number
        authorName: string
        urlPreview: string | null
        urlShow: string
        urlDownload: string
      }>
    }

    try {
      const response = await $b24.actions.v2.call.make<TimelineCommentGetResult>({
        method: 'crm.timeline.comment.get',
        params: {
          id: 999,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.ID, result.ENTITY_TYPE, result.COMMENT)
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
      async function getTimelineComment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.timeline.comment.get',
            params: {
              id: 999,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.ID, result.ENTITY_TYPE, result.COMMENT)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTimelineComment)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.timeline.comment.get(
            bitrix_id=999,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.timeline.comment.get',
                [
                    'id' => 999,
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

    ```js
    BX24.callMethod(
        "crm.timeline.comment.get",
        {
            id: 999,
        }, result => {
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
        'crm.timeline.comment.get',
        [
            'id' => 999
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.timeline.comment.get", b24.Params{
    	"id": 999,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.timeline.comment.get: %w", err)
    }

    var item struct {
    	ID         b24.ID `json:"ID"`
    	EntityID   b24.ID `json:"ENTITY_ID"`
    	EntityType string `json:"ENTITY_TYPE"`
    	Created    string `json:"CREATED"`
    	Comment    string `json:"COMMENT"`
    	AuthorID   b24.ID `json:"AUTHOR_ID"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.EntityID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": "999",
        "ENTITY_ID": "2",
        "ENTITY_TYPE": "deal",
        "CREATED": "2020-03-02T12:00:00+03:00",
        "COMMENT": "New comment was added",
        "AUTHOR_ID": "1",
        "FILES": {
            "930": {
                "id": 930,
                "date": "2020-03-02T12:00:00+03:00",
                "type": "image",
                "name": "1.gif",
                "size": 43,
                "image": {
                    "width": 1,
                    "height": 1
                },
                "authorId": 1,
                "authorName": "John Dou",
                "urlPreview": "https://my.bitrix24.com/disk/showFile/930/?&ncc=1&width=640&height=640&signature=292f450929833cd881070155e05a2c41b5bb265ea8c8c1bc2108dbcbb56f667f&ts=1718366521&filename=1.gif",
                "urlShow": "https://my.bitrix24.com/disk/showFile/930/?&ncc=1&ts=1718366521&filename=1.gif",
                "urlDownload": "https://my.bitrix24.com/disk/downloadFile/930/?&ncc=1&filename=1.gif"
            },
            "931": {
                "id": 931,
                "date": "2020-03-02T12:00:00+03:00",
                "type": "image",
                "name": "2.gif",
                "size": 43,
                "image": {
                    "width": 1,
                    "height": 1
                },
                "authorId": 1,
                "authorName": "John Dou",
                "urlPreview": "https://my.bitrix24.com/disk/showFile/931/?&ncc=1&width=640&height=640&signature=118de010a40eff06fb9d691ee9235e2ef809a17780e46927bf8b12f8dc3224db&ts=1718366521&filename=2.gif",
                "urlShow": "https://my.bitrix24.com/disk/showFile/931/?&ncc=1&ts=1718366521&filename=2.gif",
                "urlDownload": "https://my.bitrix24.com/disk/downloadFile/931/?&ncc=1&filename=2.gif"
            }
        }
    },
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-07T17:19:01+03:00",
        "date_finish": "2024-05-07T17:19:01+03:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with information about the comment [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

#### Object result {#result}

The full field description is in the [crm.timeline.comment.fields](./crm-timeline-comment-fields.md#fields) method. The `ID`, `ENTITY_ID`, and `AUTHOR_ID` identifiers are of type `integer`, but may be returned as strings in JSON.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Comment identifier ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the first linked CRM item that the user has permission to read ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | Type of the CRM item specified in `ENTITY_ID` ||
|| **CREATED**
[`datetime`](../../../data-types.md) | Date and time the comment was created ||
|| **COMMENT**
[`string`](../../../data-types.md) | Comment text ||
|| **AUTHOR_ID**
[`integer`](../../../data-types.md) | Identifier of the comment author ||
|| **FILES**
[`object`](../../../data-types.md) | Comment files. The key is the file identifier on Drive, the value is a file object [(detailed description)](#files). The field is returned if the comment has files, the Drive module is available, and data for at least one file was retrieved ||
|#

#### FILES Element {#files}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | File identifier on Drive ||
|| **date**
[`datetime`](../../../data-types.md) | Date and time the file was created ||
|| **type**
[`string`](../../../data-types.md) | Content type: `image`, `video`, or `file` ||
|| **name**
[`string`](../../../data-types.md) | File name ||
|| **size**
[`integer`](../../../data-types.md) | File size in bytes ||
|| **image**
[`object`](../../../data-types.md), [`boolean`](../../../data-types.md) | Dimensions of the image or video preview: `width` and `height` of type `integer`, in pixels. For other files — `false` ||
|| **authorId**
[`integer`](../../../data-types.md) | Identifier of the user who created the file ||
|| **authorName**
[`string`](../../../data-types.md) | Name of the user who created the file ||
|| **urlPreview**
[`string`](../../../data-types.md) or `null` | Preview link. If no preview is available — `null` ||
|| **urlShow**
[`string`](../../../data-types.md) | Link to view an image or download any other file ||
|| **urlDownload**
[`string`](../../../data-types.md) | File download link ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Not found."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | Not found. | No timeline record with the specified `id` was found ||
|| `400` | Empty value | Access denied. | No read permission for any CRM item linked to the comment, or the comment has no bindings ||
|| `400` | Empty value | ID is not defined or invalid. | The `id` parameter is not passed or contains an invalid identifier ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-comment-add.md)
- [{#T}](./crm-timeline-comment-update.md)
- [{#T}](./crm-timeline-comment-list.md)
- [{#T}](./crm-timeline-comment-delete.md)
- [{#T}](./crm-timeline-comment-fields.md)