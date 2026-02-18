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

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'booking.v1.waitlist.list',
        {
          filter: {
            createdWithin: {
              from: "01.04.2025",
              to: "16.04.2025",
            }
          }
        },
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('booking.v1.waitlist.list', {
        filter: {
          createdWithin: {
            from: "01.04.2025",
            to: "16.04.2025",
          }
        }
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('booking.v1.waitlist.list', {
        filter: {
          createdWithin: {
            from: "01.04.2025",
            to: "16.04.2025",
          }
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
                'booking.v1.waitlist.list',
                [
                    'filter' => [
                        'createdWithin' => [
                            'from' => '01.04.2025',
                            'to'   => '16.04.2025', //exclusive, records with the latest date of 15.04.2025 will be selected
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing waitlist: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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
[`object`](../../data-types.md) | Root element of the response. Contains an array of records in the waitlist. The structure is described [below](#waitList) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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
|| `0` | `Required fields: ` | Required parameter not provided within `createdWithin` ||
|| `422` | `Invalid date` | Invalid date ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-waitlist-createfrombooking.md)
- [{#T}](./booking-v1-waitlist-update.md)
- [{#T}](./booking-v1-waitlist-get.md)
- [{#T}](./booking-v1-waitlist-add.md)
- [{#T}](./booking-v1-waitlist-delete.md)

