# Get the list of bookings booking.v1.booking.list

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.list` returns a list of bookings based on a filter. It is an implementation of the list method for bookings.

## Method Parameters

#|
|| **FILTER**
[`object`](../../data-types.md) | Object for filtering the list of bookings, description of available fields [below](#filter) ||
|| **ORDER**
[`object`](../../data-types.md) | Object for sorting the list of bookings. The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending

The default value is `{id: 'ASC'}`. Description of available fields [below](#order) ||
|#

### Filter Parameters {#filter}

#|
|| **Name**
`type` | **Description** ||
|| **within**
[`object`](../../data-types.md) | Object for filtering by booking time in the format `{"dateFrom": "0", "dateTo": "1739262600"}`, where
- `dateFrom` — start of the period, a number in timestamp format
- `dateTo` — end of the period, a number in timestamp format
  
If the object is provided, all parameters within it are required ||
|| **client**
[`object`](../../data-types.md) | Object for filtering by client, accepts an array of `entities` objects with fields
- `code` — client type code
- `module` — module 
- `id` — element identifier

Available types and module for clients are returned by the method [booking.v1.clienttype.list](../booking-v1-clienttype-list.md). 

If the object is provided, all parameters within it are required ||
|#

### Order Parameters {#order}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Sort by identifier ||
|| **dateFrom**
[`string`](../../data-types.md) | Sort by start date ||
|| **dateTo**
[`string`](../../data-types.md) | Sort by end date ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.booking.list",
        {
            filter: {
                within: {
                    dateFrom: 0,
                    dateTo: 1739262600,
                },
                client: {
                    entities: [
                        {
                            "code": "CONTACT",
                            "module": "crm",
                            "id": "1"
                        },
                        {
                            "code": "COMPANY",
                            "module": "crm",
                            "id": "1"
                        }
                    ]
                }
            },
            order: {
                id: "ASC",
                dateFrom: "DESC",
                dateTo: "ASC",
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
    -d '{"filter":{"within":{"dateFrom":0,"dateTo":1739262600},"client":{"entities":[{"code":"CONTACT","module":"crm","id":"1"},{"code":"COMPANY","module":"crm","id":"1"}]}},"order":{"id":"ASC","dateFrom":"DESC","dateTo":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"within":{"dateFrom":0,"dateTo":1739262600},"client":{"entities":[{"code":"CONTACT","module":"crm","id":"1"},{"code":"COMPANY","module":"crm","id":"1"}]}},"order":{"id":"ASC","dateFrom":"DESC","dateTo":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.list
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.booking.list',
        [
            'filter' => [
                'within' => [
                    'dateFrom' => 0,
                    'dateTo' => 1739262600,
                ],
                'client' => [
                    'entities' => [
                        [
                            'code' => 'CONTACT',
                            'module' => 'crm',
                            'id' => '1'
                        ],
                        [
                            'code' => 'COMPANY',
                            'module' => 'crm',
                            'id' => '1'
                        ]
                    ]
                ]
            ],
            'order' => [
                'id' => 'ASC',
                'dateFrom' => 'DESC',
                'dateTo' => 'ASC',
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
        "booking": [
            {
                "datePeriod": {
                    "from": {
                        "timestamp": 1723446900,
                        "timezone": "Europe/Berlin"
                    },
                    "to": {
                        "timestamp": 1723447800,
                        "timezone": "Europe/Berlin"
                    }
                },
                "description": null,
                "id": 15,
                "name": "booking 15",
                "resourceIds": [
                    1,
                    2
                ]
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
[`object`](../../data-types.md) | Root element of the response. 

Contains an array of objects with information about bookings. The structure is described [below](#booking)  ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Booking {#booking}

#|
|| **datePeriod**
[`object`](../../data-types.md) | Time period of the booking. Contains fields `from` and `to` with information about the start and end times of the booking ||
|| **description**
[`string`](../../data-types.md) | Description of the booking. Can be `null` ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the booking ||
|| **name**
[`string`](../../data-types.md) | Name of the booking ||
|| **resourceIds**
[`array`](../../data-types.md) | Array of resource identifiers associated with the booking. Resource descriptions can be obtained using the method [booking.v1.resource.get](../resource/booking-v1-resource-get.md) ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 422,
    "error_description": "Invalid date period"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `1018` | `Empty resource collection` | Empty array of resources or such resources do not exist ||
|| `0` | `Required fields:` | Required parameter not provided within `filter` ||
|| `100` | `Could not find value for parameter ` | Required parameter not provided ||
|| `422` | `Invalid date period` | Incorrect time period ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-booking-createfromwaitlist.md)
- [{#T}](./booking-v1-booking-delete.md)
- [{#T}](./booking-v1-booking-add.md)
- [{#T}](./booking-v1-booking-update.md)
- [{#T}](./booking-v1-booking-get.md)