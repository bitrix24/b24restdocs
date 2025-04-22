# Add Client to Booking booking.v1.booking.client.set

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.client.set` sets clients for the specified booking.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **bookingId***
[`integer`](../../../data-types.md) | Booking identifier.
Can be obtained using the methods [booking.v1.booking.add](../booking-v1-booking-add.md) and [booking.v1.booking.list](../booking-v1-booking-list.md)  ||
|| **clients***
[`array`](../../../data-types.md) | Array of objects containing information about clients [(detailed description)](#clients) ||
|#

### Parameter clients {#clients}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Client identifier, can be obtained using the method [crm.item.list](../../../crm/universal/crm-item-list.md) for contacts and companies ||
|| **type***
[`object`](../../../data-types.md) | Client type in the format `{"module": "crm", "code": "CONTACT"}`.
Possible values for `code`:
- `CONTACT` — [CRM contact](../../../crm/contacts/index.md)
- `COMPANY` — [CRM company](../../../crm/companies/index.md)
  
The structure of the object is returned by the method [booking.v1.clienttype.list](../../booking-v1-clienttype-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.booking.client.set",
        {
            bookingId: 14,
            clients: [
                {
                    id: 1,
                    type: {
                        module: "crm",
                        code: "CONTACT"
                    }
                },
                {
                    id: 2,
                    type: {
                        module: "crm",
                        code: "CONTACT"
                    }
                }
            ]
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
    -d '{"bookingId":14,"clients":[{"id":1,"type":{"module":"crm","code":"CONTACT"}},{"id":2,"type":{"module":"crm","code":"CONTACT"}}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.client.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"bookingId":14,"clients":[{"id":1,"type":{"module":"crm","code":"CONTACT"}},{"id":2,"type":{"module":"crm","code":"CONTACT"}}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.client.set
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.booking.client.set',
        [
            'bookingId' => 14,
            'clients' => [
                [
                    'id' => 1,
                    'type' => [
                        'module' => 'crm',
                        'code' => 'CONTACT'
                    ]
                },
                [
                    'id' => 2,
                    'type' => [
                        'module' => 'crm',
                        'code' => 'CONTACT'
                    ]
                }
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
    "result": true,
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
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 1021,
    "error_description": "Booking not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Required fields:` | Required parameter not provided within `clients` ||
|| `1021` | `Booking not found` | Booking with the specified `id` not found ||
|| `100` | `Could not find value for parameter` | Required parameter not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-booking-client-unset.md)
- [{#T}](./booking-v1-booking-client-list.md)