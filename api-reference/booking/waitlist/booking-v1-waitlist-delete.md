# Delete Entry from the Waitlist booking.v1.waitlist.delete

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.waitlist.delete` removes an entry from the waitlist.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the entry in the waitlist ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.waitlist.delete",
        {
            id: 15
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.waitlist.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.waitlist.delete
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.waitlist.delete',
        [
            'id' => 15
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
"result": true,
"time": {
    "start": 1741002472.477039,
    "finish": 1741002472.598432,
    "duration": 0.12139296531677246,
    "processing": 0.012734174728393555,
    "date_start": "2025-03-03T11:47:52+00:00",
    "date_finish": "2025-03-03T11:47:52+00:00",
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` on success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 100,
    "error_description": "Could not find value for parameter {id}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Could not find value for parameter {id}` | Required parameter `id` not provided ||
|| `1036` | `wait list item not found` | Waitlist with such an ID not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-waitlist-createfrombooking.md)
- [{#T}](./booking-v1-waitlist-update.md)
- [{#T}](./booking-v1-waitlist-get.md)
- [{#T}](./booking-v1-waitlist-list.md)
- [{#T}](./booking-v1-waitlist-add.md)