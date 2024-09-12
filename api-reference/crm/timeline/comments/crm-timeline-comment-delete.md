# Delete Comment crm.timeline.comment.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method deletes a deal of type "Comment".

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Integer identifier of the deal of type "Comment" (for example, `1`). You can obtain identifiers using the method [`crm.timeline.comment.list`](./crm-timeline-comment-list.md) ||
|| **ownerTypeId**
[`integer`](../../data-types.md#object_type) | [Integer identifier of the CRM entity type](../../data-types.md#object_type) to which the comment is linked (for example, `2` for a deal) ||
|| **ownerId**
[`integer`](../../../data-types.md) | Integer identifier of the CRM entity to which the comment is linked (for example, `1`). You can get a list of identifiers using the method [`crm.timeline.bindings.list`](../bindings/crm-timeline-bindings-list.md) (field `ENTITY_ID`) ||
|#

{% note warning %}

When specifying `ownerTypeId` and `ownerId`, if the comment is linked to multiple entities, the comment will be deleted from the entity whose identifiers were provided. You can get a list of all bindings for the comment using the method [`crm.timeline.bindings.list`](../bindings/crm-timeline-bindings-list.md)

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":10}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.comment.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":10,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.comment.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.timeline.comment.delete",
        {
            id: 999,
            ownerTypeId: 2,
            ownerId: 10,
        },
        result => {
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
        'crm.timeline.comment.delete',
        [
            'id' => 999,
            'ownerTypeId' => 2,
            'ownerId' => 10
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": null,
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
`null` | The operation always returns `null` ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

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
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `NOT_FOUND` | Element not found ||
|| `MULTIPLE_BINDINGS` | Element has bindings to multiple entities ||
|| `OWNER_NOT_FOUND` | Owner of the element not found ||
|| `100` | Required fields not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-comment-add.md)
- [{#T}](./crm-timeline-comment-update.md)
- [{#T}](./crm-timeline-comment-get.md)
- [{#T}](./crm-timeline-comment-list.md)
- [{#T}](./crm-timeline-comment-fields.md)