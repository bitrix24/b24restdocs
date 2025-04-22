# Set Connections for Recording in the Waitlist booking.v1.waitlist.externalData.set

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.waitlist.externalData.set` establishes connections for the specified entry in the waitlist.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **waitListId***
[`integer`](../../../data-types.md) | Identifier of the waitlist entry. 
Can be obtained using the methods [booking.v1.waitlist.add](../booking-v1-waitlist-add.md) and [booking.v1.waitlist.list](../booking-v1-waitlist-list.md) ||
|| **externalData***
[`array`](../../../data-types.md) | Array of objects containing objects for binding [(detailed description)](#externalData) ||
|#

### Parameter externalData {#externalData}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../../../data-types.md) | Module identifier, for example `crm` ||
|| **entityTypeId***
[`string`](../../../data-types.md) | Object type ID, for example `DEAL` ||
|| **value***
[`string`](../../../data-types.md) | ID of the element, for example `1` ||
|#

Currently, connections can only be established with [deals](../../../crm/deals/index.md) in CRM.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.waitlist.externalData.set",
        {
            waitListId: 14,
            externalData: [
                {
                    moduleId: "crm",
                    entityTypeId: "DEAL",
                    value: "1"
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
    -d '{"waitListId":14,"externalData":[{"moduleId":"crm","entityTypeId":"DEAL","value":"1"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.waitlist.externalData.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":14,"externalData":[{"moduleId":"crm","entityTypeId":"DEAL","value":"1"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.waitlist.externalData.set
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.waitlist.externalData.set',
        [
            'waitListId' => 14,
            'externalData' => [
                [
                    'moduleId' => 'crm',
                    'entityTypeId' => 'DEAL',
                    'value' => '1'
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

HTTP status: **400**

```json
{
    "error": 1040,
    "error_description": "Wait list not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Required fields:` | Required parameter not provided within `externalData` ||
|| `1040` | `Wait list not found` | Waitlist with the specified `id` not found ||
|| `100` | `Could not find value for parameter` | Required parameter not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-waitlist-externaldata-unset.md)
- [{#T}](./booking-v1-waitlist-externaldata-list.md)