# Add Clients to Booking booking.v1.booking.client.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.booking.client.set` replaces the list of booking clients with the provided list.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **bookingId***
[`integer`](../../../data-types.md) | Booking identifier.

Can be obtained using the methods [booking.v1.booking.add](../booking-v1-booking-add.md) and [booking.v1.booking.list](../booking-v1-booking-list.md) ||
|| **clients***
[`array`](../../../data-types.md) | Array of booking clients [(detailed description)](#clients). The array can contain several contacts and companies.

The method replaces the entire current list: clients that are not in the array are unlinked from the booking. To add a client to those already linked, retrieve the current list using the method [booking.v1.booking.client.list](./booking-v1-booking-client-list.md) and pass it together with the new client.

An empty array `[]` removes all clients, like the method [booking.v1.booking.client.unset](./booking-v1-booking-client-unset.md). The exception is a booking linked to a deal that already has no clients: in this case, the method fills in the contacts and the company from the deal ||
|#

### Parameter clients {#clients}

{% include [Note on required parameters](../../../../_includes/required.md) %}

Pass the object keys in lowercase: `id`, `type`, `module`, `code`. The method does not recognize uppercase keys and returns the `Required fields` error.

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of an existing CRM contact or company.

You can retrieve the identifier using the method [crm.item.list](../../../crm/universal/crm-item-list.md): with `entityTypeId = 3` for contacts, with `entityTypeId = 4` for companies ||
|| **type***
[`object`](../../../data-types.md) | Client type [(detailed description)](#type) ||
|#

### Parameter type {#type}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **module***
[`string`](../../../data-types.md) | Client module. Possible value: `crm` ||
|| **code***
[`string`](../../../data-types.md) | Client type code. Possible values:
- `CONTACT` — [CRM contact](../../../crm/contacts/index.md)
- `COMPANY` — [CRM company](../../../crm/companies/index.md)

The list of available types is returned by the method [booking.v1.clienttype.list](../../booking-v1-clienttype-list.md) ||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"bookingId":14,"clients":[{"id":1,"type":{"module":"crm","code":"CONTACT"}},{"id":2,"type":{"module":"crm","code":"CONTACT"}}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.booking.client.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"bookingId":14,"clients":[{"id":1,"type":{"module":"crm","code":"CONTACT"}},{"id":2,"type":{"module":"crm","code":"CONTACT"}}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.booking.client.set
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'booking.v1.booking.client.set',
        params: {
          bookingId: 14,
          clients: [
            {
              id: 1,
              type: {
                module: 'crm',
                code: 'CONTACT',
              },
            },
            {
              id: 2,
              type: {
                module: 'crm',
                code: 'CONTACT',
              },
            },
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Clients set successfully:', result)
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
      async function setBookingClients() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'booking.v1.booking.client.set',
            params: {
              bookingId: 14,
              clients: [
                {
                  id: 1,
                  type: {
                    module: 'crm',
                    code: 'CONTACT',
                  },
                },
                {
                  id: 2,
                  type: {
                    module: 'crm',
                    code: 'CONTACT',
                  },
                },
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Clients set successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setBookingClients)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.booking.v1.booking.client.set(
            booking_id=14,
            clients=[
                {
                    "id": 1,
                    "type": {
                        "module": "crm",
                        "code": "CONTACT",
                    },
                },
                {
                    "id": 2,
                    "type": {
                        "module": "crm",
                        "code": "CONTACT",
                    },
                },
            ],
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
                'booking.v1.booking.client.set',
                [
                    'bookingId' => 14,
                    'clients'   => [
                        [
                            'id'   => 1,
                            'type' => [
                                'module' => 'crm',
                                'code'   => 'CONTACT',
                            ],
                        ],
                        [
                            'id'   => 2,
                            'type' => [
                                'module' => 'crm',
                                'code'   => 'CONTACT',
                            ],
                        ],
                    ],
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

- PHP CRest

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
                ],
                [
                    'id' => 2,
                    'type' => [
                        'module' => 'crm',
                        'code' => 'CONTACT'
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "booking.v1.booking.client.set", b24.Params{
    	"bookingId": 14,
    	"clients": []b24.Params{
    		{
    			"id": 1,
    			"type": b24.Params{
    				"module": "crm",
    				"code":   "CONTACT",
    			},
    		},
    		{
    			"id": 2,
    			"type": b24.Params{
    				"module": "crm",
    				"code":   "CONTACT",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("booking.v1.booking.client.set: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
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
    "error": "1021",
    "error_description": "Booking not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Could not find value for parameter {bookingId}` | The required parameter `bookingId` is not provided ||
|| `100` | `Could not find value for parameter {clients}` | The required parameter `clients` is not provided ||
|| `100` | `Invalid value {abc} to match with parameter {bookingId}. Should be value of type int.` | `bookingId` contains a value of a different type instead of a number ||
|| `100` | `Invalid value {abc} to match with parameter {clients}. Should be value of type array.` | `clients` is not an array ||
|| `0` | `Required fields: id, type` | An element of `clients` is missing `id`, `type`, `type.module`, or `type.code`, or the keys are passed in uppercase. The error text lists the missing fields ||
|| `1021` | `Booking not found` | Booking with the specified `bookingId` not found ||
|| `1025` | `Client type not found` | `type` contains an unknown `module` and `code` pair, for example `"code": "LEAD"` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./booking-v1-booking-client-unset.md)
- [{#T}](./booking-v1-booking-client-list.md)
- [{#T}](../../booking-v1-clienttype-list.md)