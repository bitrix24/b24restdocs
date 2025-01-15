# Delete Event calendar.event.delete

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method deletes an event.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the event.

You can obtain the identifier using the [calendar.event.get](./calendar-event-get.md) or [calendar.event.get.nearest](./calendar-event-get-nearest.md) methods ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.details.configuration.forceCommonScopeForAll
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.details.configuration.forceCommonScopeForAll
    ```

- JS

    ```js
    BX24.callMethod(
        'calendar.event.delete',
        {
            id: 698
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.details.configuration.forceCommonScopeForAll',
        []
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
        "start": 1733404941.508815,
        "finish": 1733404942.004014,
        "duration": 0.49519896507263184,
        "processing": 0.2604491710662842,
        "date_start": "2024-12-05T13:22:21+00:00",
        "date_finish": "2024-12-05T13:22:22+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the deletion was successful ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Event id not specified"
}
```
{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Event id not specified | Required parameter `id` not provided ||
|| Empty string | An error occurred while deleting the event | Another error ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}