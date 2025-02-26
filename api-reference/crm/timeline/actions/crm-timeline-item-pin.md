# Pin a Record in the Timeline crm.timeline.item.pin

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.timeline.item.pin` pins a record in the timeline.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the timeline record, for example `999`. You can obtain the id using the method [crm.timeline.comment.list](../comments/crm-timeline-comment-list.md) ||
|| **ownerTypeId***
[`integer`](../../data-types.md#object_type) | [Identifier of the CRM object type](../../data-types.md#object_type) to which the record is linked, for example `2` for a deal ||
|| **ownerId***
[`integer`](../../../data-types.md) | Identifier of the CRM entity to which the record is linked, for example `10` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id": 999, "ownerTypeId": 2, "ownerId": 10}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.item.pin
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":10,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.item.pin
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.timeline.item.pin",
        {
            id: 999,
            ownerTypeId: 2,
            ownerId: 10,
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
        'crm.timeline.item.pin',
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

HTTP Status: **200**

```json
{
    "result": null,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`null`](../../../data-types.md) | Result of the operation. Always returns `null` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
|| `0` | Only three events can be added to favorites ||
|| `100` | Required fields are missing ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `NOT_FOUND` | Item not found ||
|| `OWNER_NOT_FOUND` | Owner of the item not found ||
|| `CAN_NOT_CHANGE_PINNED` | Unable to perform the operation ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-item-unpin.md)