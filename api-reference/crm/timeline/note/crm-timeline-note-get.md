# Get Information About Note crm.timeline.note.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method returns information about a note related to a timeline record.

## Method Parameters

{% include [Footnote about required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerTypeId***
[`integer`](../../../data-types.md) | [Identifier of the entity type](../../data-types.md) to which the record belongs ||
|| **ownerId***
[`integer`](../../../data-types.md) | Identifier of the entity to which the record belongs ||
|| **itemType***
[`integer`](../../../data-types.md) | Type of the record to which the note should be applied: 

- `1` — history record
- `2` — deal ||
|| **itemId***
[`integer`](../../../data-types.md) | Identifier of the record to which the note should be applied. If `itemType=1`, this is the identifier of the timeline history record. If `itemType=2`, this is the identifier of the deal ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerTypeId":1,"ownerId":1,"itemType":1,"itemId":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.note.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerTypeId":1,"ownerId":1,"itemType":1,"itemId":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.note.get
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.timeline.note.get",
        {
            ownerTypeId: 1,
            ownerId: 1,
            itemType: 1,
            itemId: 2,
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
        'crm.timeline.note.get',
        [
            'ownerTypeId' => 1,
            'ownerId' => 1,
            'itemType' => 1,
            'itemId' => 2
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
        "text": "Test note",
        "createdById": 1,
        "createdTime": "2024-03-17T15:55:10+03:00",
        "updatedById": 1,
        "updatedTime": "2024-03-17T15:55:10+03:00"
    },
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
[`object`](../../../data-types.md) | Information about the found note:

- **text** — text of the note
- **createdById** — identifier of the user who created the note
- **createdTime** — date and time the note was created
- **updatedById** — identifier of the user who modified the note
- **updatedTime** — date and time the note was modified ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Element not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `NOT_FOUND` | Element not found ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-note-delete.md)
- [{#T}](./crm-timeline-note-save.md)