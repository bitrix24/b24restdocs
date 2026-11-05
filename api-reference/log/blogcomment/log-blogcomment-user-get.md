# Get User Comments log.blogcomment.user.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `log.blogcomment.user.get` returns a user's comments on News Feed posts and information about the files attached to them.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID**
[`integer`](../../data-types.md) | The identifier of the user whose comments need to be retrieved. If the parameter is not passed or is passed by a non-administrator, the comments of the current user are returned.

Only an administrator can retrieve another user's comments. For these comments, the `text` field is empty unless the comment is a system comment, in which case it contains the system text.

The user identifier can be obtained using the [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md) methods. ||
|| **FIRST_ID**
[`integer`](../../data-types.md) | The method returns comments whose `id` is greater than this value. Do not pass `comment_id` here: it is a different identifier ||
|| **LAST_ID**
[`integer`](../../data-types.md) | The method returns comments whose `id` is less than this value. If `FIRST_ID` is also passed, the method uses only `FIRST_ID` ||
|| **LIMIT**
[`integer`](../../data-types.md) | How many comments to return, from `1` to `1000`. The default is `100`. The method replaces a larger value with `1000` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":28,"FIRST_ID":215}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogcomment.user.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":28,"FIRST_ID":215,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogcomment.user.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BlogCommentUserGetResult = {
      COMMENTS: {
        id: number
        comment_id: number
        post_id: number
        date: ISODate
        text: string
        attach: number[]
      }[]
      // An empty array is returned when no files are attached
      FILES: [] | Record<string, {
        id: number
        date: ISODate
        type: string
        name: string
        size: number
        // Image or video preview size; false for other files
        image: {
          width: number
          height: number
        } | false
        authorId: number
        authorName: string
        urlPreview: string | null
        urlShow: string
        urlDownload: string
      }>
    }

    try {
      const response = await $b24.actions.v2.call.make<BlogCommentUserGetResult>({
        method: 'log.blogcomment.user.get',
        params: {
          USER_ID: 28,
          FIRST_ID: 215,
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
      async function getBlogCommentsForUser() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'log.blogcomment.user.get',
            params: {
              USER_ID: 28,
              FIRST_ID: 215,
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

      document.addEventListener('DOMContentLoaded', getBlogCommentsForUser)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.log.blogcomment.user.get(
            user_id=28,
            first_id=215,
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
                'log.blogcomment.user.get',
                [
                    'USER_ID' => 28,
                    'FIRST_ID' => 215,
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
    BX24.callMethod('log.blogcomment.user.get', {
        USER_ID: 28,
        FIRST_ID: 215
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
        'log.blogcomment.user.get',
        [
            'USER_ID'  => 28,
            'FIRST_ID' => 215,
        ]
    );

    if (array_key_exists('error', $result)) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Comments: ' . print_r($result['result'], true);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "log.blogcomment.user.get", b24.Params{
    	"USER_ID":  28,
    	"FIRST_ID": 215,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("log.blogcomment.user.get: %w", err)
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
                "id": 4821,
                "comment_id": 4821,
                "post_id": 13579,
                "date": "2024-10-14T11:05:42+02:00",
                "text": "I support the idea!",
                "attach": [
                    90210
                ]
            }
        ],
        "FILES": {
            "90210": {
                "id": 90210,
                "date": "2024-10-14T11:02:18+02:00",
                "type": "image",
                "name": "diagram.png",
                "size": 184320,
                "image": {
                    "width": 1280,
                    "height": 720
                },
                "authorId": 28,
                "authorName": "John Smith",
                "urlPreview": "https://example.bitrix24.com/disk/showPreview/90210",
                "urlShow": "https://example.bitrix24.com/disk/showFile/90210",
                "urlDownload": "https://example.bitrix24.com/disk/downloadFile/90210"
            }
        }
    },
    "time": {
        "start": 1728905100.112233,
        "finish": 1728905100.498321,
        "duration": 0.38608813285827637,
        "processing": 0.14598798751831055,
        "date_start": "2024-10-14T14:25:00+02:00",
        "date_finish": "2024-10-14T14:25:00+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root object of the response. Contains an array of comments and information about attached files [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **COMMENTS**
[`object[]`](../../data-types.md) | Array of the user's comments, from newest to oldest: in descending order of `comment_id`. The element structure is described [below](#comments) ||
|| **FILES**
[`object`](../../data-types.md) \| [`array`](../../data-types.md) | Files attached to the comments, in the format `{"<file identifier>": {…}}`. The key is the Drive file identifier from the comment's `attach` array. The file structure is described [below](#files).

If there are no files, an empty array `[]` is returned ||
|#

#### COMMENTS Array Element {#comments}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the comment record in the News Feed. This is the value passed in `FIRST_ID` and `LAST_ID` ||
|| **comment_id**
[`integer`](../../data-types.md) | Identifier of the comment itself. Pass it to the [log.blogcomment.delete](./log-blogcomment-delete.md) method ||
|| **post_id**
[`integer`](../../data-types.md) | Identifier of the News Feed post the comment was left on ||
|| **date**
[`datetime`](../../data-types.md) | Comment creation date and time in ISO 8601 format ||
|| **text**
[`string`](../../data-types.md) | Comment text. If an administrator retrieves another user's comments, the text is empty, except for system comments ||
|| **attach**
[`integer[]`](../../data-types.md) | Identifiers of the Drive files attached to the comment. The files are described in the `FILES` object ||
|#

#### File Object in FILES {#files}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Drive file identifier ||
|| **date**
[`datetime`](../../data-types.md) | File upload date and time in ISO 8601 format ||
|| **type**
[`string`](../../data-types.md) | File type: `image` — image, `video` — video, `file` — other file ||
|| **name**
[`string`](../../data-types.md) | File name ||
|| **size**
[`integer`](../../data-types.md) | File size in bytes ||
|| **image**
[`object`](../../data-types.md) \| [`boolean`](../../data-types.md) | Dimensions of the image or video preview: `width` and `height` — width and height in pixels, integers. For other files, `false` ||
|| **authorId**
[`integer`](../../data-types.md) | Identifier of the user who uploaded the file ||
|| **authorName**
[`string`](../../data-types.md) | Name of the user who uploaded the file ||
|| **urlPreview**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Link to the file preview. If there is no preview, `null` is returned ||
|| **urlShow**
[`string`](../../data-types.md) | Link to view the image. For other files, it matches the download link ||
|| **urlDownload**
[`string`](../../data-types.md) | Link to download the file ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./log-blogcomment-add.md)
- [{#T}](./log-blogcomment-get.md)
- [{#T}](./log-blogcomment-delete.md)
