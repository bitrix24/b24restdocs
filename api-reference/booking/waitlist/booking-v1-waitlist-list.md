# Get a list of records from the waitlist booking.v1.waitlist.list

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.waitlist.list` returns a list of records from the waitlist based on the filter.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering waitlist records. 
Pass the `createdWithin` object inside the filter to filter by creation date, [(detailed description)](#createdWithin)  ||
|#

### Parameter createdWithin {#createdWithin}

#|
|| **from*** 
[`string`](../../data-types.md) | Start date of the filtering period in the format `dd.mm.yyyy`, inclusive ||
|| **to*** 
[`string`](../../data-types.md) | End date of the filtering period in the format `dd.mm.yyyy`, exclusive ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.waitlist.list",
        {
            filter: {
                createdWithin: {
                    from: "01.04.2025", 
                    to: "16.04.2025", //exclusive, records with the latest date of 15.04.2025 will be selected
                }
            }
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
    -d '{"filter":{"createdWithin":{"from":"01.04.2025","to":"16.04.2025"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.waitlist.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"createdWithin":{"from":"01.04.2025","to":"16.04.2025"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.waitlist.list
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.waitlist.list',
        [
            'filter' => [
                'createdWithin' => [
                    'from' => '01.04.2025',
                    'to' => '16.04.2025' //exclusive, records with the latest date of 15.04.2025 will be selected
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

HTTP status: **200**

```json
{
    "result": {
        "waitList": [
            {
                "id": 15,
                "note": "note"
            },
            {
                "id": 16,
                "note": "note"
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
[`object`](../../data-types.md) | The root element of the response. Contains an array of records in the waitlist. The structure is described [below](#waitList) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Record in the waitlist {#waitList} 

#|
|| **id**
[`integer`](../../data-types.md) | Identifier of the record in the waitlist ||
|| **note**
[`string`](../../data-types.md) | Note associated with the record in the waitlist. Can be `null` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 422,
    "error_description": "Invalid date"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Required fields: ` | A required parameter inside `createdWithin` was not provided ||
|| `422` | `Invalid date` | Invalid date ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-waitlist-createfrombooking.md)
- [{#T}](./booking-v1-waitlist-update.md)
- [{#T}](./booking-v1-waitlist-get.md)
- [{#T}](./booking-v1-waitlist-add.md)
- [{#T}](./booking-v1-waitlist-delete.md)