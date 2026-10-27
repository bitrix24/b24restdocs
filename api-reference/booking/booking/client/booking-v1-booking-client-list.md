# Get the list of booking clients booking.v1.booking.client.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.client.list` returns a list of clients for the specified booking.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **bookingId***
[`integer`](../../../data-types.md) | Booking identifier.

Can be obtained using the methods [booking.v1.booking.add](../booking-v1-booking-add.md) and [booking.v1.booking.list](../booking-v1-booking-list.md) ||
|#

{% note info "" %}

The method returns all clients of the booking in a single response. It has no pagination, filtering, or sorting, and ignores the `start` parameter.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"bookingId":123}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.client.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"bookingId":123,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.client.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BookingClientListResult = {
      bookingClient: {
        id: number
        type: {
          code: string
          module: string
        }
      }[]
    }

    // booking.v1.booking.client.list returns all clients of the booking in one response:
    // the method has no pagination and ignores `start`, so a single call.make is enough
    try {
      const response = await $b24.actions.v2.call.make<BookingClientListResult>({
        method: 'booking.v1.booking.client.list',
        params: {
          bookingId: 123,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Booking clients:', result.bookingClient, 'Count:', result.bookingClient.length)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listBookingClients() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // booking.v1.booking.client.list returns all clients of the booking in one response:
          // the method has no pagination and ignores `start`, so a single call.make is enough
          const response = await $b24.actions.v2.call.make({
            method: 'booking.v1.booking.client.list',
            params: {
              bookingId: 123,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Booking clients:', result.bookingClient, 'Count:', result.bookingClient.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listBookingClients)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.booking.v1.booking.client.list(
            booking_id=123,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'booking.v1.booking.client.list',
                [
                    'bookingId' => 123,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.booking.client.list",
        {
            bookingId: 123,
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
        'booking.v1.booking.client.list',
        [
            'bookingId' => 123
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "booking.v1.booking.client.list", b24.Params{
    	"bookingId": 123,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("booking.v1.booking.client.list: %w", err)
    }

    // The method wraps the response in an object with the "bookingClient" key.
    raw, ok := b24.Unwrap(res.Result, "bookingClient")
    if !ok {
    	return fmt.Errorf("no bookingClient key in the response")
    }

    var items []struct {
    	ID b24.ID `json:"id"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "bookingClient": [
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
    "total": 0,
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
[`object`](../../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **total**
[`integer`](../../../data-types.md) | Service field, always `0`. Count the clients by the length of the `bookingClient` array ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **bookingClient**
[`array`](../../../data-types.md) | Array of booking clients [(detailed description)](#bookingClient). Clients are listed in the order in which they were passed to [booking.v1.booking.client.set](./booking-v1-booking-client-set.md).

If the booking has no clients, an empty array `[]` is returned ||
|#

#### Object bookingClient {#bookingClient}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the CRM contact or company. Client data can be retrieved using the method [crm.item.get](../../../crm/universal/crm-item-get.md): with `entityTypeId = 3` for a contact, with `entityTypeId = 4` for a company ||
|| **type**
[`object`](../../../data-types.md) | Client type [(detailed description)](#type) ||
|#

#### Object type {#type}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../../data-types.md) | Client type code:
- `CONTACT` — [CRM contact](../../../crm/contacts/index.md)
- `COMPANY` — [CRM company](../../../crm/companies/index.md) ||
|| **module**
[`string`](../../../data-types.md) | Client module. Possible value: `crm` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "1021",
    "error_description": "Booking not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Could not find value for parameter {bookingId}` | The required parameter `bookingId` is not provided ||
|| `100` | `Invalid value {abc} to match with parameter {bookingId}. Should be value of type int.` | `bookingId` contains a value of a different type instead of a number ||
|| `1021` | `Booking not found` | Booking with the specified `bookingId` not found ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./booking-v1-booking-client-set.md)
- [{#T}](./booking-v1-booking-client-unset.md)