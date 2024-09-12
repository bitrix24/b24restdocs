# Get Information About Comment crm.timeline.comment.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method retrieves information about a deal of type "Comment".

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Integer identifier of the deal of type "Comment" (for example, `1`). You can obtain identifiers using the [`crm.timeline.comment.list`](./crm-timeline-comment-list.md) method ||
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

- JS

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

- PHP

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

{% endlist %}

## Response Handling

HTTP Status: **200**

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
            "1": {
                "id": 1,
                "date": "2020-03-02T12:00:00+03:00",
                "type": "image",
                "name": "1.gif",
                "size": 43,
                "image": {
                    "width": 1,
                    "height": 1
                },
                "authorId": 1,
                "authorName": "John Doe",
                "urlPreview": "https://my.bitrix24.com/disk/showFile/930/?&ncc=1&width=640&height=640&signature=292f450929833cd881070155e05a2c41b5bb265ea8c8c1bc2108dbcbb56f667f&ts=1718366521&filename=1.gif",
                "urlShow": "https://my.bitrix24.com/disk/showFile/930/?&ncc=1&ts=1718366521&filename=1.gif",
                "urlDownload": "https://my.bitrix24.com/disk/downloadFile/930/?&ncc=1&filename=1.gif"
            },
            "2": {
                "id": 2,
                "date": "2020-03-02T12:00:00+03:00",
                "type": "image",
                "name": "2.gif",
                "size": 43,
                "image": {
                    "width": 1,
                    "height": 1
                },
                "authorId": 1,
                "authorName": "John Doe",
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
        "date_start": "2024-05-03T17:19:01+03:00",
        "date_finish": "2024-05-03T17:19:01+03:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The root element of the response. The values for the `result` field correspond to the fields of the [result](./crm-timeline-comment-fields.md#field-result) object. ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Not found. | The element with the specified parameters was not found ||
|| Empty string | Access denied. | No rights to edit the entity in CRM ||
|| Empty string | ID is not defined or invalid. | Required fields were not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-comment-add.md)
- [{#T}](./crm-timeline-comment-update.md)
- [{#T}](./crm-timeline-comment-list.md)
- [{#T}](./crm-timeline-comment-delete.md)
- [{#T}](./crm-timeline-comment-fields.md)