# Get the list of client types booking.v1.clienttype.list

> Scope: [`booking`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.clienttype.list` returns a list of available client types. The method may not return a client type if no bookings have been created with it. By default, there are two client types available: contact and company.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.clienttype.list",
        {},
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
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.clienttype.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth": "**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.clienttype.list
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.clienttype.list',
        []
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
    "result": {
        "clientType": [
            {
                "code": "COMPANY",
                "module": "crm"
            },
            {
                "code": "CONTACT",
                "module": "crm"
            }
        ]
    },
    "time": {
     "start": 1724068028.331234,
     "finish": 1724068028.726591,
     "duration": 0.3953571319580078,
     "processing": 0.13033390045166016,
     "date_start": "2025-01-21T13:47:08+02:00",
     "date_finish": "2025-01-21T13:47:08+02:00",
     "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | An object containing the list of client types ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)