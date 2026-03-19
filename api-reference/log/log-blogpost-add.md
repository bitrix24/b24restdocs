# Add a Message to the News Feed log.blogpost.add

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `log.blogpost.add` adds a message to the News Feed.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_MESSAGE***
[`string`](../data-types.md) | Message text ||
|| **POST_TITLE**
[`string`](../data-types.md) | Message title ||
|| **DEST**
[`array`](../data-types.md) | List of recipients who will have permission to view the message.

Possible values:

{% include notitle [Message Recipients](./_includes/log-recepients.md) %}

Default — `UA`
||
|| **SPERM**
[`array`](../data-types.md) | Deprecated equivalent of `DEST` ||
|| **FILES**
[`array`](../data-types.md) | Array of files in the format described in [working with files](../files/how-to-upload-files.md).

Files will be uploaded to the author's Drive and linked to the message ||
|| **IMPORTANT**
[`string`](../data-types.md) | Indicator of an important message.

Possible values:

- `Y` — message is important
- `N` — message is not important

Default — `N` ||
|| **IMPORTANT_DATE_END**
[`string`](../data-types.md) | Date and time in ISO 8601 format until which the message will be considered important ||
|| **SITE_ID**
[`string`](../data-types.md) | Site identifier.

Default — current site ||
|| **USER_ID**
[`integer`](../data-types.md) | Identifier of the user on behalf of whom the message is published. Available only to administrators.

The identifier can be obtained using the [user.get](../user/user-get.md) method.

Default — current user who initiated the method call ||
|| **TAGS**
[`string`](../data-types.md) | Message tags ||
|| **BACKGROUND_CODE**
[`string`](../data-types.md) | Background code of the message ||
|| **PARSE_PREVIEW**
[`string`](../data-types.md) | Automatic addition of a link preview from the message text.

Possible values:
- `Y` — attempt to generate a link preview from `POST_MESSAGE`
- `N` — do not generate a preview

Default — `N` ||
|| **UF_\***
[`mixed`](../data-types.md) | Custom fields. A specific [set of fields](#uf-fields) is supported, depending on the account settings ||
|#

### Custom Fields {#uf-fields}

#|
|| **Name**
`type` | **Description** ||
|| **UF_BLOG_POST_FILE**
[`array`](../data-types.md) | Alternative to `FILES`.

Pass a list of Drive file identifiers in the format `['n<ID_drive_file>']`.

The identifier can be obtained using the [disk.storage.getchildren](../disk/storage/disk-storage-get-children.md) and [disk.folder.getchildren](../disk/folder/disk-folder-get-children.md) methods.

{% note info "" %}

When specifying `FILES`, the `UF_BLOG_POST_FILE` parameter is ignored.

{% endnote %}  ||
|| **UF_BLOG_POST_IMPRTNT**
[`integer`](../data-types.md) | Indicator of an important message.

Automatically filled when `IMPORTANT = 'Y'` ||
|| **UF_IMPRTANT_DATE_END**
[`datetime`](../data-types.md#datetime) | Expiration date of the important message.

Automatically filled when `IMPORTANT_DATE_END` is provided ||
|| **UF_BLOG_POST_URL_PRV**
[`integer`](../data-types.md) | Link preview from the message text.

Automatically filled when `PARSE_PREVIEW = 'Y'`, if the preview was successfully generated ||
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
            FIELD_TYPE: 0, // Selection type: 0 — one option, 1 — multiple options
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
When creating a new survey, use a random identifier with the prefix `n` ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_TITLE":"New Regulation","POST_MESSAGE":"From November 1, the approval process is updated.","DEST":["UA"],"TAGS":"regulation,approval,update","IMPORTANT":"Y","FILES":[["first-image.jpg","iVBORw0KGgoAAAANSUhEUgAAAAUA..."],["second-image.jpg","iVBORw0KGgoAAAANSUhEUgAAAAUA..."]]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_TITLE":"New Regulation","POST_MESSAGE":"From November 1, the approval process is updated.","DEST":["UA"],"TAGS":"regulation,approval,update","IMPORTANT":"Y","FILES":[["first-image.jpg","iVBORw0KGgoAAAANSUhEUgAAAAUA..."],["second-image.jpg","iVBORw0KGgoAAAANSUhEUgAAAAUA..."]],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogpost.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'log.blogpost.add',
            {
                POST_TITLE: 'New Regulation',
                POST_MESSAGE: 'From November 1, the approval process is updated.',
                DEST: ['UA'],
                TAGS: 'regulation,approval,update',
                IMPORTANT: 'Y',
                FILES: [
                    ['first-image.jpg', 'iVBORw0KGgoAAAANSUhEUgAAAAUA...'],
                    ['second-image.jpg', 'iVBORw0KGgoAAAANSUhEUgAAAAUA...']
                ]
            }
        );
        
        const result = response.getData().result;
        console.log('Created element with ID:', result);
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
                'log.blogpost.add',
                [
                    'POST_TITLE' => 'New Regulation',
                    'POST_MESSAGE' => 'From November 1, the approval process is updated.',
                    'DEST' => ['UA'],
                    'TAGS' => 'regulation,approval,update',
                    'IMPORTANT' => 'Y',
                    'FILES' => [
                        ['first-image.jpg', 'iVBORw0KGgoAAAANSUhEUgAAAAUA...'],
                        ['second-image.jpg', 'iVBORw0KGgoAAAANSUhEUgAAAAUA...']
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
        echo 'Error adding blog post: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'log.blogpost.add',
        {
            POST_TITLE: 'New Regulation',
            POST_MESSAGE: 'From November 1, the approval process is updated.',
            DEST: ['UA'],
            TAGS: 'regulation,approval,update',
            IMPORTANT: 'Y',
            FILES: [
                ['first-image.jpg', 'iVBORw0KGgoAAAANSUhEUgAAAAUA...'],
                ['second-image.jpg', 'iVBORw0KGgoAAAANSUhEUgAAAAUA...']
            ]
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
        'log.blogpost.add',
        [
            'POST_TITLE' => 'New Regulation',
            'POST_MESSAGE' => 'From November 1, the approval process is updated.',
            'DEST' => ['UA'],
            'TAGS' => 'regulation,approval,update',
            'IMPORTANT' => 'Y',
            'FILES' => [
                ['first-image.jpg', 'iVBORw0KGgoAAAANSUhEUgAAAAUA...'],
                ['second-image.jpg', 'iVBORw0KGgoAAAANSUhEUgAAAAUA...']
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
        "start": 1773750554,
        "finish": 1773750555.955794,
        "duration": 1.955794095993042,
        "processing": 1,
        "date_start": "2026-03-17T15:29:14+01:00",
        "date_finish": "2026-03-17T15:29:15+01:00",
        "operating_reset_at": 1773751154,
        "operating": 0.9908020496368408
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the created message ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "SONET_CONTROLLER_LIVEFEED_BLOGPOST_ADD_ERROR",
    "error_description": "Blog post hasn't been added"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `SONET_CONTROLLER_LIVEFEED_BLOGPOST_ADD_ERROR` | `Blog post hasn't been added` | General error saving the message, for example, when `POST_MESSAGE` is empty ||
|| `SONET_CONTROLLER_LIVEFEED_BLOGPOST_ADD_ERROR` | `No destination specified` | Failed to determine the message recipients ||
|| `SONET_CONTROLLER_LIVEFEED_BLOGPOST_MODULE_BLOG_NOT_INSTALLED` | `Blog module is not installed` | The `blog` module is not installed ||
|| `SONET_CONTROLLER_LIVEFEED_BLOG_NOT_FOUND` | `Blog not found` | Failed to retrieve the blog to which the message belongs ||
|| — | `Cannot add blog post` | Internal error when creating the message ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./log-blogpost-update.md)
- [{#T}](./log-blogpost-get.md)
- [{#T}](./log-blogpost-delete.md)
- [{#T}](./log-blogpost-share.md)
- [{#T}](./log-blogpost-getusers-important.md)