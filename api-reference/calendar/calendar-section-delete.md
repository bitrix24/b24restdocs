# Delete Calendar calendar.section.delete

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method deletes a calendar.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../data-types.md) | Calendar type: 
- `user` — user calendar
- `group` — group calendar ||
|| **ownerId***
[`integer`](../data-types.md) | Identifier of the calendar owner ||
|| **id***
[`integer`](../data-types.md) | Identifier of the calendar ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":2,"id":521}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.section.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":2,"id":521,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.section.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'calendar.section.delete',
        {
            type: 'user',
            ownerId: 2,
            id: 521
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.section.delete',
        [
            'type' => 'user',
            'ownerId' => 2,
            'id' => 521
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
    "result": true,
    "time": {
        "start": 1733822629.623482,
        "finish": 1733822629.878903,
        "duration": 0.2554209232330322,
        "processing": 0.049089908599853516,
        "date_start": "2024-12-08T09:23:49+00:00",
        "date_finish": "2024-12-08T09:23:49+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Result of the calendar deletion.

Returns `true` if the calendar was successfully deleted ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "type" for the method "calendar.section.delete" is not set"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "type" for the method "calendar.section.delete" is not set | The required parameter `type` was not provided ||
|| Empty string | The required parameter "ownerId" for the method "calendar.section.add" is not set | The required parameter `ownerId` was not provided and the `type` parameter is not equal to `user` ||
|| Empty string | Section id is not set | The required parameter `id` was not provided ||
|| Empty string | Access denied | The calendar with the specified `id` does not exist or there are no permissions to edit the calendar ||
|| Empty string | An error occurred while deleting the section | Another error ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-section-add.md)
- [{#T}](./calendar-section-update.md)
- [{#T}](./calendar-settings-get.md)