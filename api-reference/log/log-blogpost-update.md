# Update News Feed Message log.blogpost.update

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: administrator or message author

The method `log.blogpost.update` updates a message in the News Feed.

## Method Parameters

{% include [Footnote on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_ID***
[`integer`](../data-types.md) | Identifier of the message.

You can obtain the identifier using the [log.blogpost.get](./log-blogpost-get.md) method. ||
|| **POST_MESSAGE**
[`string`](../data-types.md) | New text of the message. ||
|| **POST_TITLE**
[`string`](../data-types.md) | New title of the message.

{% note info "" %}

If neither `POST_TITLE` nor `POST_MESSAGE` is provided, the method will return an error `EMPTY_TITLE`. If only `POST_MESSAGE` is provided, the title from the original message will not be retained.

{% endnote %} ||
|| **DEST**
[`array`](../data-types.md) | New list of recipients who will have the right to view the message.

Possible values:

{% include notitle [message recipients](./_includes/log-recepients.md) %}
||
|| **SPERM**
[`array`](../data-types.md) | Deprecated equivalent of `DEST`. ||
|| **FILES**
[`array`](../data-types.md) | Array of files in the format described in [working with files](../files/how-to-upload-files.md).

To delete a file, use the format `{ ID: 'del' }`, where `ID` is the identifier of the file attachment in the message.

You can obtain the identifier using the [log.blogpost.get](./log-blogpost-get.md) method. ||
|| **IMPORTANT**
[`string`](../data-types.md) | Indicator of an important message.

Possible values:

- `Y` — message is important
- `N` — message is not important ||
|| **IMPORTANT_DATE_END**
[`string`](../data-types.md) | Date and time in ISO 8601 format until which the message will be considered important. ||
|| **SITE_ID**
[`string`](../data-types.md) | Identifier of the site. ||
|| **USER_ID**
[`integer`](../data-types.md) | Identifier of the user on behalf of whom the message is edited. Available only to administrators.

The identifier can be obtained using the [user.get](../user/user-get.md) method.

By default, it is the current user who initiated the method call. ||
|| **UF_\***
[`mixed`](../data-types.md) | Custom fields. A specific [set of fields](#uf-fields) is supported, depending on the account settings. ||
|#

### Custom Fields {#uf-fields}

#|
|| **Name**
`type` | **Description** ||
|| **UF_BLOG_POST_FILE**
[`array`](../data-types.md) | Alternative to `FILES`.

Pass a list of Drive file identifiers in the format `['n<ID_drive_file>']`.

You can obtain the identifier using the [disk.storage.getchildren](../disk/storage/disk-storage-get-children.md) and [disk.folder.getchildren](../disk/folder/disk-folder-get-children.md) methods.

To clear the list of files from the message, pass `['empty']`. The files will not be physically deleted from the Drive; only the attachment to the message will be removed.

{% note info "" %}

When specifying `FILES`, the `UF_BLOG_POST_FILE` parameter is ignored.

{% endnote %} ||
|| **UF_BLOG_POST_IMPRTNT**
[`integer`](../data-types.md) | Indicator of an important message.

Automatically filled when `IMPORTANT = 'Y'`. ||
|| **UF_IMPRTANT_DATE_END**
[`datetime`](../data-types.md#datetime) | Expiration date of the important message.

Automatically filled when `IMPORTANT_DATE_END` is provided. ||
|| **UF_BLOG_POST_URL_PRV**
[`integer`](../data-types.md) | Preview link from the message text.

Automatically filled when `PARSE_PREVIEW = 'Y'`, if the preview was successfully generated. ||
|| **UF_GRATITUDE**
[`integer`](../data-types.md) | Data for the Gratitude functionality in the format:

```js
 GRATITUDE_MEDAL: '<XML_ID_medal>',
 GRATITUDE_EMPLOYEES: [<user_ID>]
```
||
|| **UF_BLOG_POST_VOTE**
[`integer`](../data-types.md) | Survey data in the format:

```js
UF_BLOG_POST_VOTE: 'n<ID_survey>',
'UF_BLOG_POST_VOTE_n<ID_survey>_DATA': {
    QUESTIONS: [
        {
            QUESTION: 'Question',
            FIELD_TYPE: 0, // Selection type: 0 — single option, 1 — multiple options
            ANSWERS: [
                { MESSAGE: 'Answer 1' },
                { MESSAGE: 'Answer 2' }
            ]
        }
    ],
    ANONYMITY: 0, // Voting anonymity: 0 — no, 1 — yes
    OPTIONS: 0 // Re-voting: 0 — prohibited, 1 — allowed
}
```
You can obtain the identifier of an existing survey using the [log.blogpost.get](./log-blogpost-get.md) method. ||
|#

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":217,"POST_TITLE":"New Message Title","FILES":{"505":"del"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":217,"POST_TITLE":"New Message Title","FILES":{"505":"del"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogpost.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'log.blogpost.update',
            {
                POST_ID: 217,
                POST_TITLE: 'New Message Title',
                FILES: {
                    505: 'del'
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Updated post with ID:', result);
        
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'log.blogpost.update',
                [
                    'POST_ID' => 217,
                    'POST_TITLE' => 'New Message Title',
                    'FILES' => [
                        505 => 'del'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating blog post: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'log.blogpost.update',
        {
            POST_ID: 217,
            POST_TITLE: 'New Message Title',
            FILES: {
                505: 'del'
            }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'log.blogpost.update',
        [
            'POST_ID' => 217,
            'POST_TITLE' => 'New Message Title',
            'FILES' => [
                505 => 'del'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 217,
    "time": {
        "start": 1773817731,
        "finish": 1773817731.902982,
        "duration": 0.9029819965362549,
        "processing": 0,
        "date_start": "2026-03-18T10:08:51+01:00",
        "date_finish": "2026-03-18T10:08:51+01:00",
        "operating_reset_at": 1773818331,
        "operating": 0.704833984375
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the updated message. ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "EMPTY_TITLE",
    "error_description": "Message title is not specified."
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `SONET_CONTROLLER_LIVEFEED_BLOGPOST_UPDATE_ERROR` | `Wrong post ID` | Invalid `POST_ID`. ||
|| `EMPTY_TITLE` | `Message title is not specified.` | Neither `POST_TITLE` nor `POST_MESSAGE` is specified. ||
|| `SONET_CONTROLLER_LIVEFEED_BLOGPOST_UPDATE_ERROR` | `Blog module is not installed.` | The `blog` module is not installed. ||
|| `SONET_CONTROLLER_LIVEFEED_BLOGPOST_UPDATE_ERROR` | `No write perms` | Insufficient permissions to modify the message. ||
|| `SONET_CONTROLLER_LIVEFEED_BLOGPOST_UPDATE_ERROR` | `No post found` | Message not found. ||
|| `SONET_CONTROLLER_LIVEFEED_BLOGPOST_UPDATE_ERROR` | `No blog found` | Unable to retrieve the author's blog. ||
|| — | `Cannot update blog post` | Internal error while updating the message. ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./log-blogpost-add.md)
- [{#T}](./log-blogpost-get.md)
- [{#T}](./log-blogpost-delete.md)
- [{#T}](./log-blogpost-share.md)
- [{#T}](./log-blogpost-getusers-important.md)