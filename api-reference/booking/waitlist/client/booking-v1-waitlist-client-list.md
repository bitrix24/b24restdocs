# Get the list of clients for the waitlist booking.v1.waitlist.client.list

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.waitlist.client.list` returns a list of clients for the specified entry in the waitlist.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **waitListId***
[`integer`](../../../data-types.md) | Identifier of the entry in the waitlist. 
Can be obtained using the methods [booking.v1.waitlist.add](../booking-v1-waitlist-add.md) and [booking.v1.waitlist.list](../booking-v1-waitlist-list.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "booking.v1.waitlist.client.list",
        {
            waitListId: 257,
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
    -d '{"waitListId":257}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.waitlist.client.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":257,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.waitlist.client.list
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'booking.v1.waitlist.client.list',
        [
            'waitListId' => 257
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
        "waitListClient": [
            {
                "id": 1,
                "type": {
                    "code": "COMPANY",
                    "module": "crm"
                }
            },
            {
                "id": 2,
                "type": {
                    "code": "CONTACT",
                    "module": "crm"
                }
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
[`object`](../../../data-types.md) | Root element of the response. Contains an array of objects with client information. The structure is described [below](#waitListClient) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Client {#waitListClient}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the client, which can be used to retrieve client data using the method [crm.item.get](../../../crm/universal/crm-item-get.md) for contacts and companies ||
|| **type**
[`object`](../../../data-types.md) | Type of the client in the format `{"module": "crm", "code": "CONTACT"}`.
Values of `code`: 
- `CONTACT` — [CRM contact](../../../crm/contacts/index.md)
- `COMPANY` — [CRM company](../../../crm/companies/index.md)

The structure of the object is returned by the method [booking.v1.clienttype.list](../../booking-v1-clienttype-list.md) ||
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
|| `1040` | `Wait list not found` | The waitlist with the specified `id` was not found ||
|| `100` | `Could not find value for parameter` | A required parameter was not provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-waitlist-client-unset.md)
- [{#T}](./booking-v1-waitlist-client-set.md)