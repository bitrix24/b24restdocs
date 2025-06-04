# Add Comment crm.timeline.comment.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method adds a new activity of type "Comment" to the timeline.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for adding a new activity of type "Comment" in the form of a structure:

```json

fields:
{
    "ENTITY_ID": 'value',
    "ENTITY_TYPE_ID": 'value',
    "COMMENT": 'value',
    "AUTHOR_ID": 'value',
    "FILES": [
        [
            "file name", 
            "file content"
        ],
        [
            "file name",
            "file content"
        ],
    ]
}
```

The file content is transmitted as a base64 string.

{% note warning %}

Starting from crm version 23.100.0, the method only accepts parameters with the key `fields` in lowercase. Other undocumented variants (Fields, FIELDS, arFields) are not accepted.

{% endnote %}

 ||
|#

### Parameter fields {#parametr-fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | `ID` of the element to which the comment is attached.

The value can be obtained using the [`crm.item.list`](../../universal/crm-item-list.md) method or when creating an element with the help of [`crm.item.add`](../../universal/crm-item-add.md) ||
|| **ENTITY_TYPE_ID***
[`string`](../../../data-types.md) | Identifier of the [system](../../index.md) or [user-defined type](../../universal/user-defined-object-types/index.md) of the CRM object to which the comment is attached. For example: `lead`, `deal`, `contact`, `company`, `order` ||
|| **AUTHOR_ID**
[`user`](../../../data-types.md#standart-objects) | Identifier of the user adding the comment ||
|| **COMMENT***
[`string`](../../../data-types.md) | Text of the comment ||
|| **FILES**
[`attached_diskfile`](../../../data-types.md) | List of files. An array of values described according to [the rules](../../../bx24-js-sdk/how-to-call-rest-methods/files.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":10,"ENTITY_TYPE_ID":"deal","COMMENT":"New comment was added","AUTHOR_ID":5,"FILES":[["1.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],["2.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.comment.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":10,"ENTITY_TYPE_ID":"deal","COMMENT":"New comment was added","AUTHOR_ID":5,"FILES":[["1.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],["2.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.comment.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.timeline.comment.add",
        {
            fields:
            {
                "ENTITY_ID": 10,
                "ENTITY_TYPE_ID": "deal",
                "COMMENT": "New comment was added",
                "AUTHOR_ID": 5,
                "FILES": [
                    [
                        "1.gif", 
                        "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="
                    ],
                    [
                        "2.gif",
                        "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="
                    ],
                ]
            }
        }, result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.comment.add',
        [
            'fields' => [
                'ENTITY_ID' => 10,
                'ENTITY_TYPE_ID' => 'deal',
                'COMMENT' => 'New comment was added',
                'AUTHOR_ID' => 5,
                'FILES' => [
                    ["1.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],
                    ["2.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]
                ]
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
    "result": 999,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+02:00",
        "date_finish": "2024-05-03T17:19:01+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Returns the integer identifier of the added comment ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `OWNER_NOT_FOUND` | Owner of the element not found ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `NOT_FOUND` | Element not found ||
|| `INVALID_ARG_VALUE` | Empty comment ||
|| `100` | Required fields not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-comment-update.md)
- [{#T}](./crm-timeline-comment-get.md)
- [{#T}](./crm-timeline-comment-list.md)
- [{#T}](./crm-timeline-comment-delete.md)
- [{#T}](./crm-timeline-comment-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-comment-to-spa.md)
