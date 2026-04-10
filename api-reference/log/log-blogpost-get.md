# Access Available User Messages from the News Feed log.blogpost.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `log.blogpost.get` returns messages from the News Feed that are accessible to the current user.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_ID**
[`integer`](../data-types.md) | Filter by message ID.

You can obtain the ID using the [log.blogpost.get](./log-blogpost-get.md) method. ||
|| **LOG_RIGHTS**
[`array`](../data-types.md) | Filter by recipients who have the right to view the message.

Possible values:

{% include notitle [Message Recipients](./_includes/log-recepients.md) %}
||
|| **start**
[`integer`](../data-types.md) | This parameter is used for pagination control.

The page size of results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` is the desired page number. ||
|#

{% note info "" %}

If the `POST_ID` and `LOG_RIGHTS` parameters are not specified, all messages available to the current user are returned. The parameters are mutually exclusive: if `POST_ID` is specified, filtering by `LOG_RIGHTS` is ignored.

{% endnote %}

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":217}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":217,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogpost.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'log.blogpost.get',
            {
                POST_ID: 217
            }
        );
        
        const result = response.getData().result;
        console.log('Blog post data:', result);
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
                'log.blogpost.get',
                [
                    'POST_ID' => 217
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving blog post: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'log.blogpost.get',
        {
            POST_ID: 217
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
        'log.blogpost.get',
        [
            'POST_ID' => 217
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

Example response:

```json
{
    "result": [
        {
        "ID": "217",
        "BLOG_ID": "299",
        "PUBLISH_STATUS": "P",
        "TITLE": "New Regulations",
        "AUTHOR_ID": "1269",
        "ENABLE_COMMENTS": "Y",
        "NUM_COMMENTS": "0",
        "CODE": null,
        "MICRO": "N",
        "DETAIL_TEXT": "The approval process will be updated starting November 1.",
        "DATE_PUBLISH": "2026-03-17T15:29:15+01:00",
        "CATEGORY_ID": "9,11,13",
        "HAS_SOCNET_ALL": "N",
        "HAS_TAGS": "Y",
        "HAS_IMAGES": "N",
        "HAS_PROPS": "Y",
        "HAS_COMMENT_IMAGES": null,
        "UF_BLOG_POST_DOC": {
            "ID": "1",
            "ENTITY_ID": "BLOG_POST",
            "FIELD_NAME": "UF_BLOG_POST_DOC",
            "USER_TYPE_ID": "file",
            "XML_ID": "UF_BLOG_POST_DOC",
            "SORT": "100",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "N",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "Y",
            "SETTINGS": {
            "SIZE": 20,
            "LIST_WIDTH": 0,
            "LIST_HEIGHT": 0,
            "MAX_SHOW_SIZE": 0,
            "MAX_ALLOWED_SIZE": 0,
            "EXTENSIONS": [],
            "TARGET_BLANK": "Y",
            "DEFAULT_VIEW": null
            },
            "EDIT_FORM_LABEL": null,
            "LIST_COLUMN_LABEL": null,
            "LIST_FILTER_LABEL": null,
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null,
            "USER_TYPE": {
            "USER_TYPE_ID": "file",
            "CLASS_NAME": "Bitrix\Main\UserField\Types\FileType",
            "EDIT_CALLBACK": ["Bitrix\Main\UserField\Types\FileType", "renderEdit"],
            "VIEW_CALLBACK": ["Bitrix\Main\UserField\Types\FileType", "renderView"],
            "USE_FIELD_COMPONENT": true,
            "DESCRIPTION": "File",
            "BASE_TYPE": "file"
            },
            "VALUE": false,
            "ENTITY_VALUE_ID": 217,
            "VALUE_EXISTS": true,
            "VALUE_RAW": null,
            "CUSTOM_DATA": []
        },
        "UF_BLOG_POST_URL_PRV": {
            "ID": "5",
            "ENTITY_ID": "BLOG_POST",
            "FIELD_NAME": "UF_BLOG_POST_URL_PRV",
            "USER_TYPE_ID": "url_preview",
            "XML_ID": "UF_BLOG_POST_URL_PRV",
            "SORT": "100",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "N",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "Y",
            "SETTINGS": [],
            "EDIT_FORM_LABEL": null,
            "LIST_COLUMN_LABEL": null,
            "LIST_FILTER_LABEL": null,
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null,
            "USER_TYPE": {
            "USER_TYPE_ID": "url_preview",
            "CLASS_NAME": "Bitrix\Main\UrlPreview\UrlPreviewUserType",
            "DESCRIPTION": "Link preview content",
            "BASE_TYPE": "int"
            },
            "VALUE": null,
            "ENTITY_VALUE_ID": 217,
            "VALUE_EXISTS": true,
            "VALUE_RAW": null,
            "CUSTOM_DATA": []
        },
        "UF_GRATITUDE": {
            "ID": "9",
            "ENTITY_ID": "BLOG_POST",
            "FIELD_NAME": "UF_GRATITUDE",
            "USER_TYPE_ID": "integer",
            "XML_ID": "UF_GRATITUDE",
            "SORT": "100",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "N",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
            "SIZE": 20,
            "MIN_VALUE": 0,
            "MAX_VALUE": 0,
            "DEFAULT_VALUE": null
            },
            "EDIT_FORM_LABEL": null,
            "LIST_COLUMN_LABEL": null,
            "LIST_FILTER_LABEL": null,
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null,
            "USER_TYPE": {
            "USER_TYPE_ID": "integer",
            "CLASS_NAME": "Bitrix\Main\UserField\Types\IntegerType",
            "EDIT_CALLBACK": ["Bitrix\Main\UserField\Types\IntegerType", "renderEdit"],
            "VIEW_CALLBACK": ["Bitrix\Main\UserField\Types\IntegerType", "renderView"],
            "USE_FIELD_COMPONENT": true,
            "DESCRIPTION": "Integer",
            "BASE_TYPE": "int"
            },
            "VALUE": null,
            "ENTITY_VALUE_ID": 217,
            "VALUE_EXISTS": true,
            "VALUE_RAW": null,
            "CUSTOM_DATA": []
        },
        "UF_BLOG_POST_FILE": {
            "ID": "19",
            "ENTITY_ID": "BLOG_POST",
            "FIELD_NAME": "UF_BLOG_POST_FILE",
            "USER_TYPE_ID": "disk_file",
            "XML_ID": "UF_BLOG_POST_FILE",
            "SORT": "100",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "N",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "Y",
            "SETTINGS": {
            "IBLOCK_ID": null,
            "SECTION_ID": null,
            "UF_TO_SAVE_ALLOW_EDIT": false
            },
            "EDIT_FORM_LABEL": null,
            "LIST_COLUMN_LABEL": null,
            "LIST_FILTER_LABEL": null,
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null,
            "USER_TYPE": {
            "USER_TYPE_ID": "disk_file",
            "CLASS_NAME": "Bitrix\Disk\Uf\FileUserType",
            "DESCRIPTION": "File (Drive)",
            "BASE_TYPE": "int",
            "TAG": ["DISK FILE ID", "DOCUMENT ID"]
            },
            "VALUE": [505],
            "ENTITY_VALUE_ID": 217,
            "VALUE_EXISTS": true,
            "VALUE_RAW": "a:1:{i:0;i:505;}",
            "CUSTOM_DATA": {
            "PHOTO_TEMPLATE": "gallery"
            }
        },
        "UF_BLOG_POST_IMPRTNT": {
            "ID": "83",
            "ENTITY_ID": "BLOG_POST",
            "FIELD_NAME": "UF_BLOG_POST_IMPRTNT",
            "USER_TYPE_ID": "integer",
            "XML_ID": "UF_BLOG_POST_IMPRTNT",
            "SORT": "100",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
            "SIZE": 20,
            "MIN_VALUE": 0,
            "MAX_VALUE": 0,
            "DEFAULT_VALUE": null
            },
            "EDIT_FORM_LABEL": "Important Message",
            "LIST_COLUMN_LABEL": "Important",
            "LIST_FILTER_LABEL": "Important",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null,
            "USER_TYPE": {
            "USER_TYPE_ID": "integer",
            "CLASS_NAME": "Bitrix\Main\UserField\Types\IntegerType",
            "EDIT_CALLBACK": ["Bitrix\Main\UserField\Types\IntegerType", "renderEdit"],
            "VIEW_CALLBACK": ["Bitrix\Main\UserField\Types\IntegerType", "renderView"],
            "USE_FIELD_COMPONENT": true,
            "DESCRIPTION": "Integer",
            "BASE_TYPE": "int"
            },
            "VALUE": null,
            "ENTITY_VALUE_ID": 217,
            "VALUE_EXISTS": true,
            "VALUE_RAW": null,
            "CUSTOM_DATA": []
        },
        "UF_IMPRTANT_DATE_END": {
            "ID": "85",
            "ENTITY_ID": "BLOG_POST",
            "FIELD_NAME": "UF_IMPRTANT_DATE_END",
            "USER_TYPE_ID": "datetime",
            "XML_ID": "UF_IMPRTANT_DATE_END",
            "SORT": "100",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "N",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
            "DEFAULT_VALUE": {
                "TYPE": "NONE",
                "VALUE": ""
            },
            "USE_SECOND": "Y",
            "USE_TIMEZONE": "N"
            },
            "EDIT_FORM_LABEL": "Expiration Date",
            "LIST_COLUMN_LABEL": "Expiration",
            "LIST_FILTER_LABEL": null,
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null,
            "USER_TYPE": {
            "USER_TYPE_ID": "datetime",
            "CLASS_NAME": "Bitrix\Main\UserField\Types\DateTimeType",
            "EDIT_CALLBACK": ["Bitrix\Main\UserField\Types\DateTimeType", "renderEdit"],
            "VIEW_CALLBACK": ["Bitrix\Main\UserField\Types\DateTimeType", "renderView"],
            "USE_FIELD_COMPONENT": true,
            "DESCRIPTION": "Date with time",
            "BASE_TYPE": "datetime"
            },
            "VALUE": "",
            "ENTITY_VALUE_ID": 217,
            "VALUE_EXISTS": true,
            "VALUE_RAW": null,
            "CUSTOM_DATA": []
        },
        "UF_BLOG_POST_VOTE": {
            "ID": "131",
            "ENTITY_ID": "BLOG_POST",
            "FIELD_NAME": "UF_BLOG_POST_VOTE",
            "USER_TYPE_ID": "vote",
            "XML_ID": "UF_BLOG_POST_VOTE",
            "SORT": "100",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
            "CHANNEL_ID": 1,
            "UNIQUE": 8,
            "UNIQUE_IP_DELAY": {
                "DELAY": "10",
                "DELAY_TYPE": "D"
            },
            "NOTIFY": "I"
            },
            "EDIT_FORM_LABEL": null,
            "LIST_COLUMN_LABEL": null,
            "LIST_FILTER_LABEL": null,
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null,
            "USER_TYPE": {
            "USER_TYPE_ID": "vote",
            "CLASS_NAME": "Bitrix\Vote\Uf\VoteUserType",
            "DESCRIPTION": "Poll",
            "BASE_TYPE": "int"
            },
            "VALUE": null,
            "ENTITY_VALUE_ID": 217,
            "VALUE_EXISTS": true,
            "VALUE_RAW": null,
            "CUSTOM_DATA": []
        },
        "UF_MAIL_MESSAGE": {
            "ID": "439",
            "ENTITY_ID": "BLOG_POST",
            "FIELD_NAME": "UF_MAIL_MESSAGE",
            "USER_TYPE_ID": "mail_message",
            "XML_ID": "",
            "SORT": "100",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "N",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": null,
            "EDIT_FORM_LABEL": null,
            "LIST_COLUMN_LABEL": null,
            "LIST_FILTER_LABEL": null,
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null,
            "USER_TYPE": {
            "USER_TYPE_ID": "mail_message",
            "CLASS_NAME": "Bitrix\Mail\MessageUserType",
            "DESCRIPTION": "Email",
            "BASE_TYPE": "int",
            "VIEW_CALLBACK": ["Bitrix\Mail\MessageUserType", "getPublicView"],
            "EDIT_CALLBACK": ["Bitrix\Mail\MessageUserType", "getPublicEdit"],
            "onBeforeSave": ["Bitrix\Mail\MessageUserType", "onBeforeSave"],
            "onDelete": ["Bitrix\Mail\MessageUserType", "onDelete"]
            },
            "VALUE": null,
            "ENTITY_VALUE_ID": 217,
            "VALUE_EXISTS": true,
            "VALUE_RAW": null,
            "CUSTOM_DATA": []
        },
        "FILES": [505]
        }
    ],
    "total": 1,
    "time": {
        "start": 1773754540,
        "finish": 1773754540.902101,
        "duration": 0.9021010398864746,
        "processing": 0,
        "date_start": "2026-03-17T16:35:40+01:00",
        "date_finish": "2026-03-17T16:35:40+01:00",
        "operating_reset_at": 1773755140,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Parameters of the message or a list of messages from the News Feed.

An empty array means there are no records that meet the filter criteria. ||
|| **ID**
[`integer`](../data-types.md) | Message ID. ||
|| **BLOG_ID**
[`integer`](../data-types.md) | ID of the blog to which the message belongs. ||
|| **PUBLISH_STATUS**
[`string`](../data-types.md) | Publication status of the message. ||
|| **TITLE**
[`string`](../data-types.md) | Title of the message. ||
|| **AUTHOR_ID**
[`integer`](../data-types.md) | ID of the message author. ||
|| **ENABLE_COMMENTS**
[`string`](../data-types.md) | Are comments allowed? ||
|| **NUM_COMMENTS**
[`integer`](../data-types.md) | Number of comments. ||
|| **CODE**
[`string`](../data-types.md) | Symbolic code of the message. ||
|| **MICRO**
[`string`](../data-types.md) | Indicator of a micro-message. ||
|| **DETAIL_TEXT**
[`string`](../data-types.md) | Text of the message. ||
|| **DATE_PUBLISH**
[`datetime`](../data-types.md#datetime) | Date and time of publication. ||
|| **CATEGORY_ID**
[`string`](../data-types.md) | Comma-separated IDs of tags (categories). ||
|| **HAS_SOCNET_ALL**
[`string`](../data-types.md) | Indicator of publication for all authorized users. ||
|| **HAS_TAGS**
[`string`](../data-types.md) | Indicator of the presence of tags. ||
|| **HAS_IMAGES**
[`string`](../data-types.md) | Indicator of the presence of images. ||
|| **HAS_PROPS**
[`string`](../data-types.md) | Indicator of the presence of custom fields. ||
|| **HAS_COMMENT_IMAGES**
[`string`](../data-types.md) | Indicator of the presence of images in comments. ||
|| **UF_\***
[`object`](../data-types.md) | Custom fields of the message. The set of fields depends on the account settings. The format of each field is described [below](#result-uf-object). ||
|| **UF_BLOG_POST_DOC**
[`object`](../data-types.md) | Additional field with files. ||
|| **UF_BLOG_POST_URL_PRV**
[`object`](../data-types.md) | Link preview data from the message text, if the preview was successfully generated. ||
|| **UF_GRATITUDE**
[`object`](../data-types.md) | Service field for the Gratitude functionality. ||
|| **UF_BLOG_POST_FILE**
[`object`](../data-types.md) | Drive files attached to the message. ||
|| **UF_BLOG_POST_IMPRTNT**
[`object`](../data-types.md) | Indicator of an important message. ||
|| **UF_IMPRTANT_DATE_END**
[`object`](../data-types.md) | Date and time until which the message is considered important. ||
|| **UF_BLOG_POST_VOTE**
[`object`](../data-types.md) | Poll data if a poll is attached to the message. ||
|| **UF_MAIL_MESSAGE**
[`object`](../data-types.md) | Connection of the message with email. ||
|| **FILES**
[`array`](../data-types.md) | Array of file IDs from `UF_BLOG_POST_FILE`. ||
|| **next**
[`integer`](../data-types.md) | Value for pagination (if available). ||
|| **total**
[`integer`](../data-types.md) | Total number of found items. ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request. ||
|#

#### Object UF_* {#result-uf-object}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | ID of the custom field. ||
|| **ENTITY_ID**
[`string`](../data-types.md) | Object of the field. ||
|| **FIELD_NAME**
[`string`](../data-types.md) | Code of the custom field. ||
|| **USER_TYPE_ID**
[`string`](../data-types.md) | Type of the custom field. ||
|| **XML_ID**
[`string`](../data-types.md) | External code of the field. ||
|| **SORT**
[`integer`](../data-types.md) | Sort order of the field. ||
|| **MULTIPLE**
[`string`](../data-types.md) | Indicator of a multiple field. ||
|| **MANDATORY**
[`string`](../data-types.md) | Indicator of mandatory. ||
|| **SHOW_FILTER**
[`string`](../data-types.md) | Indicator of displaying the field in the filter. ||
|| **SHOW_IN_LIST**
[`string`](../data-types.md) | Indicator of displaying the field in lists. ||
|| **EDIT_IN_LIST**
[`string`](../data-types.md) | Indicator of editing the field in lists. ||
|| **IS_SEARCHABLE**
[`string`](../data-types.md) | Indicator of the field's participation in search. ||
|| **SETTINGS**
[`object`](../data-types.md) | Field settings depending on the type. ||
|| **EDIT_FORM_LABEL**
[`string`](../data-types.md) | Label of the field in the edit form. ||
|| **LIST_COLUMN_LABEL**
[`string`](../data-types.md) | Label of the field in list columns. ||
|| **LIST_FILTER_LABEL**
[`string`](../data-types.md) | Label of the field in the filter. ||
|| **ERROR_MESSAGE**
[`string`](../data-types.md) | Error message of the field. ||
|| **HELP_MESSAGE**
[`string`](../data-types.md) | Help message for the field. ||
|| **USER_TYPE**
[`object`](../data-types.md) | Description of the [custom field type](#result-uf-user-type). ||
|| **VALUE**
[`string`](../data-types.md) | Value of the field. ||
|| **ENTITY_VALUE_ID**
[`integer`](../data-types.md) | ID of the message to which the field value belongs. ||
|| **VALUE_EXISTS**
[`boolean`](../data-types.md) | Indicator of the presence of the field value. ||
|| **VALUE_RAW**
[`string`](../data-types.md) | Raw value of the field. ||
|| **CUSTOM_DATA**
[`object`](../data-types.md) | Additional data of the field. ||
|#

#### Object USER_TYPE {#result-uf-user-type}

#|
|| **Name**
`type` | **Description** ||
|| **USER_TYPE_ID**
[`string`](../data-types.md) | Identifier of the custom field type. ||
|| **CLASS_NAME**
[`string`](../data-types.md) | PHP class of the field type handler. ||
|| **EDIT_CALLBACK**
[`array`](../data-types.md) | Handler that forms the edit interface of the field. ||
|| **VIEW_CALLBACK**
[`array`](../data-types.md) | Handler that forms the display of the field in the view interface. ||
|| **USE_FIELD_COMPONENT**
[`boolean`](../data-types.md) | Indicator of using the standard field component. ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Name of the field type. ||
|| **BASE_TYPE**
[`string`](../data-types.md) | Base type of the value. ||
|| **TAG**
[`array`](../data-types.md) | Additional tags of the field type. ||
|#

## Error Handling

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./log-blogpost-add.md)
- [{#T}](./log-blogpost-update.md)
- [{#T}](./log-blogpost-delete.md)
- [{#T}](./log-blogpost-share.md)
- [{#T}](./log-blogpost-getusers-important.md)