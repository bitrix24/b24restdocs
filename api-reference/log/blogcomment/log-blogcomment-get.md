# Get Post Comments log.blogcomment.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to the post

The method `log.blogcomment.get` returns published comments on a News Feed post and information about the files attached to them.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_ID***
[`integer`](../../data-types.md) | News Feed post identifier.

The post identifier can be obtained using the [log.blogpost.get](../log-blogpost-get.md) method ||
|| **FIRST_ID**
[`integer`](../../data-types.md) | The method returns comments with identifiers greater than the specified value. Comments are sorted by identifier in ascending order.

If you pass `0`, the method returns comments from the beginning of the list in ascending order of identifier.

Cannot be passed together with `LAST_ID` ||
|| **LAST_ID**
[`integer`](../../data-types.md) | The method returns comments with identifiers less than the specified value. Comments are sorted by identifier in descending order.

Cannot be passed together with `FIRST_ID` ||
|| **LIMIT**
[`integer`](../../data-types.md) | The number of comments in the response. The maximum value is `1000`; a larger value is replaced with `1000`. If the value is not passed or is less than `1`, the method returns `100` comments ||
|#

If you pass neither `FIRST_ID` nor `LAST_ID`, the method returns the latest comments in descending order of identifier. To get the next batch, pass the smallest `id` from the previous response in `LAST_ID`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":319,"LAST_ID":62,"LIMIT":3}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogcomment.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":319,"LAST_ID":62,"LIMIT":3,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogcomment.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BlogCommentGetResult = {
      COMMENTS: {
        id: number
        comment_id: number
        post_id: number
        parent_id: number
        author_id: number
        date: ISODate
        text: string
        attach: number[]
      }[]
      // An empty array is returned when no files are attached
      FILES: [] | Record<string, {
        id: number
        date: ISODate
        type: 'file' | 'image' | 'video'
        name: string
        size: number
        image: { width: number, height: number } | false
        authorId: number
        authorName: string
        urlPreview: string | null
        urlShow: string
        urlDownload: string
      }>
    }

    try {
      const response = await $b24.actions.v2.call.make<BlogCommentGetResult>({
        method: 'log.blogcomment.get',
        params: {
          POST_ID: 319,
          LAST_ID: 62,
          LIMIT: 3,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Comments:', result.COMMENTS, 'Files:', result.FILES)
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
      async function getBlogComments() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'log.blogcomment.get',
            params: {
              POST_ID: 319,
              LAST_ID: 62,
              LIMIT: 3,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Comments:', result.COMMENTS, 'Files:', result.FILES)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getBlogComments)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.log.blogcomment.get(
            post_id=319,
            last_id=62,
            limit=3,
        ).response
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
                'log.blogcomment.get',
                [
                    'POST_ID' => 319,
                    'LAST_ID' => 62,
                    'LIMIT'   => 3,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('log.blogcomment.get', {
        POST_ID: 319,
        LAST_ID: 62,
        LIMIT: 3
    },
    function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'log.blogcomment.get',
        [
            'POST_ID' => 319,
            'LAST_ID' => 62,
            'LIMIT'   => 3,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Comments: ' . print_r($result['result'], true);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "log.blogcomment.get", b24.Params{
    	"POST_ID": 319,
    	"LAST_ID": 62,
    	"LIMIT":   3,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("log.blogcomment.get: %w", err)
    }

    // The response shape is shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "COMMENTS": [
            {
                "id": 61,
                "comment_id": 61,
                "post_id": 319,
                "parent_id": 0,
                "author_id": 28,
                "date": "2026-10-21T16:38:05+02:00",
                "text": "The process diagram is attached",
                "attach": [
                    10885
                ]
            },
            {
                "id": 59,
                "comment_id": 59,
                "post_id": 319,
                "parent_id": 0,
                "author_id": 28,
                "date": "2026-10-21T16:37:30+02:00",
                "text": "I support the idea",
                "attach": [
                    10883
                ]
            },
            {
                "id": 57,
                "comment_id": 57,
                "post_id": 319,
                "parent_id": 0,
                "author_id": 28,
                "date": "2026-10-21T16:37:27+02:00",
                "text": "First comment",
                "attach": []
            }
        ],
        "FILES": {
            "10883": {
                "id": 10883,
                "date": "2026-10-21T16:37:55+02:00",
                "type": "file",
                "name": "notes.txt",
                "size": 37,
                "image": false,
                "authorId": 28,
                "authorName": "Klaus Weber",
                "urlPreview": null,
                "urlShow": "https://test.bitrix24.com/disk/downloadFile/10883/?&ncc=1&filename=notes.txt",
                "urlDownload": "https://test.bitrix24.com/disk/downloadFile/10883/?&ncc=1&filename=notes.txt"
            },
            "10885": {
                "id": 10885,
                "date": "2026-10-21T16:38:20+02:00",
                "type": "image",
                "name": "diagram.png",
                "size": 70,
                "image": {
                    "width": 1,
                    "height": 1
                },
                "authorId": 28,
                "authorName": "Klaus Weber",
                "urlPreview": "https://test.bitrix24.com/disk/showFile/10885/?&ncc=1&width=640&height=640&signature=**signature**&ts=1792589900&filename=diagram.png",
                "urlShow": "https://test.bitrix24.com/disk/showFile/10885/?&ncc=1&ts=1792589900&filename=diagram.png",
                "urlDownload": "https://test.bitrix24.com/disk/downloadFile/10885/?&ncc=1&filename=diagram.png"
            }
        }
    },
    "time": {
        "start": 1792589984,
        "finish": 1792589984.324943,
        "duration": 0.32494306564331055,
        "processing": 0,
        "date_start": "2026-10-21T16:39:44+02:00",
        "date_finish": "2026-10-21T16:39:44+02:00",
        "operating_reset_at": 1792590540,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root object of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **COMMENTS**
[`object[]`](../../data-types.md) | Array of comments [(detailed description)](#comments). If there are no comments or the `blog` module is not installed, an empty array is returned ||
|| **FILES**
[`object`](../../data-types.md) | Files attached to the comments. The key is the Drive file identifier, and the value is the file description [(detailed description)](#files). If there are no files, an empty array `[]` is returned ||
|#

#### COMMENTS Array Element {#comments}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Comment identifier ||
|| **comment_id**
[`integer`](../../data-types.md) | Comment identifier, matches `id` ||
|| **post_id**
[`integer`](../../data-types.md) | News Feed post identifier ||
|| **parent_id**
[`integer`](../../data-types.md) | Parent comment identifier ||
|| **author_id**
[`integer`](../../data-types.md) | Comment author identifier ||
|| **date**
[`datetime`](../../data-types.md) | Comment creation date and time in ISO 8601 format ||
|| **text**
[`string`](../../data-types.md) | Comment text. For system comments, the text displayed in the Feed is returned ||
|| **attach**
[`integer[]`](../../data-types.md) | Identifiers of the Drive files attached to the comment. The description of each file is in the `FILES` object under this identifier ||
|#

#### File Object in FILES {#files}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Drive file identifier ||
|| **date**
[`datetime`](../../data-types.md) | File creation date and time in ISO 8601 format ||
|| **type**
[`string`](../../data-types.md) | File type: `image`, `video`, or `file` ||
|| **name**
[`string`](../../data-types.md) | File name ||
|| **size**
[`integer`](../../data-types.md) | File size in bytes ||
|| **image**
[`object`](../../data-types.md) \| [`boolean`](../../data-types.md) | Image or video dimensions in pixels: `width` and `height`. For other files, `false` is returned ||
|| **authorId**
[`integer`](../../data-types.md) | Identifier of the user who uploaded the file ||
|| **authorName**
[`string`](../../data-types.md) | Name of the user who uploaded the file ||
|| **urlPreview**
[`string`](../../data-types.md) | Link to a preview up to 640×640 pixels. For a file without a preview that is not an image, `null` is returned ||
|| **urlShow**
[`string`](../../data-types.md) | Link to view the image. For other files, a download link ||
|| **urlDownload**
[`string`](../../data-types.md) | Link to download the file ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "POST_ID must be a positive integer"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_ARGUMENT` | POST_ID must be a positive integer | `POST_ID` is not passed, or a value less than `1` is passed ||
|| `400` | `ERROR_ARGUMENT` | FIRST_ID and LAST_ID cannot be used together | `FIRST_ID` and `LAST_ID` are passed at the same time ||
|| `404` | `ERROR_NOT_FOUND` | Post not found | The post with the passed `POST_ID` was not found, or the user does not have read access to it ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./log-blogcomment-add.md)
- [{#T}](./log-blogcomment-user-get.md)
- [{#T}](./log-blogcomment-delete.md)
