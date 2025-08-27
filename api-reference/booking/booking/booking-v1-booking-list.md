# Get the list of bookings booking.v1.booking.list

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.list` returns a list of bookings based on the filter. It is an implementation of the listing method for bookings.

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

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'booking.v1.booking.list',
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
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative fetching using a generator, allowing data to be processed in chunks and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('booking.v1.booking.list', {
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
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('booking.v1.booking.list', {
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
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'booking.v1.booking.list',
                [
                    'filter' => [
                        'within' => [
                            'dateFrom' => 0,
                            'dateTo'   => 1739262600,
                        ],
                        'client' => [
                            'entities' => [
                                [
                                    'code'   => 'CONTACT',
                                    'module' => 'crm',
                                    'id'     => '1',
                                ],
                                [
                                    'code'   => 'COMPANY',
                                    'module' => 'crm',
                                    'id'     => '1',
                                ],
                            ],
                        ],
                    ],
                    'order' => [
                        'id'       => 'ASC',
                        'dateFrom' => 'DESC',
                        'dateTo'   => 'ASC',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching booking list: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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
                        },
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
[`time`](../../data-types.md#time) | Information about the time taken to execute the request ||
|#

#### Booking {#booking}

#|
|| **datePeriod**
[`object`](../../data-types.md) | Period of time for the booking. Contains fields `from` and `to` with information about the start and end times of the booking ||
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