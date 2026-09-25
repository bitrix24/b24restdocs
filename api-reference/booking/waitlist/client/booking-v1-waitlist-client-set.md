# Set Clients for a Waitlist Entry booking.v1.waitlist.client.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Sets the list of clients for the specified waitlist entry. Clients can be CRM contacts and companies.

{% note warning "" %}

The method replaces the entry's entire client list. To keep the current clients, retrieve them using the [booking.v1.waitlist.client.list](./booking-v1-waitlist-client-list.md) method and pass them in `clients` along with the new ones.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **waitListId***
[`integer`](../../../data-types.md) | Identifier of the waitlist entry.
Can be obtained using the methods [booking.v1.waitlist.add](../booking-v1-waitlist-add.md) and [booking.v1.waitlist.list](../booking-v1-waitlist-list.md) ||
|| **clients***
[`array`](../../../data-types.md) | Complete list of the entry's clients. Each element is an object with the `id` and `type` fields. [Element Structure](#clients), [Empty Array Behavior](#empty-clients) ||
|#

### Clients Parameter {#clients}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of a CRM contact or company. You can retrieve it using the [crm.item.list](../../../crm/universal/crm-item-list.md) method: `entityTypeId: 3` for contacts, `entityTypeId: 4` for companies ||
|| **type***
[`object`](../../../data-types.md) | Client type. For example, `{"module": "crm", "code": "CONTACT"}`. [Object Structure](#client-type) ||
|#

### type Object {#client-type}

#|
|| **Name**
`type` | **Description** ||
|| **module***
[`string`](../../../data-types.md) | Client module. For contacts and companies, `crm` ||
|| **code***
[`string`](../../../data-types.md) | Client type code:
- `CONTACT` — [CRM contact](../../../crm/contacts/index.md)
- `COMPANY` — [CRM company](../../../crm/companies/index.md)

The available client types are returned by the [booking.v1.clienttype.list](../../booking-v1-clienttype-list.md) method ||
|#

### Empty clients Array {#empty-clients}

To remove the current client links, pass `clients: []`.

{% note warning "Linked Deal" %}

If the entry no longer has clients but a deal is linked to it using the [booking.v1.waitlist.externalData.set](../external-data/booking-v1-waitlist-externaldata-set.md) method, a call with `clients: []` links the contacts and company of that deal to the entry. As a result, a repeated call can populate the list again. Check the result using the [booking.v1.waitlist.client.list](./booking-v1-waitlist-client-list.md) method.

{% endnote %}

## Code Examples

The examples link entry `13` to contact `2795` and company `3063`. Replace the identifiers with values from your Bitrix24.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":13,"clients":[{"id":2795,"type":{"module":"crm","code":"CONTACT"}},{"id":3063,"type":{"module":"crm","code":"COMPANY"}}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.waitlist.client.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":13,"clients":[{"id":2795,"type":{"module":"crm","code":"CONTACT"}},{"id":3063,"type":{"module":"crm","code":"COMPANY"}}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.waitlist.client.set
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
        method: 'booking.v1.waitlist.client.set',
        params: {
          waitListId: 13,
          clients: [
            {
              id: 2795,
              type: {
                module: 'crm',
                code: 'CONTACT',
              },
            },
            {
              id: 3063,
              type: {
                module: 'crm',
                code: 'COMPANY',
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
      async function setWaitlistClients() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'booking.v1.waitlist.client.set',
            params: {
              waitListId: 13,
              clients: [
                {
                  id: 2795,
                  type: {
                    module: 'crm',
                    code: 'CONTACT',
                  },
                },
                {
                  id: 3063,
                  type: {
                    module: 'crm',
                    code: 'COMPANY',
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

      document.addEventListener('DOMContentLoaded', setWaitlistClients)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.booking.v1.waitlist.client.set(
            wait_list_id=13,
            clients=[
                {
                    "id": 2795,
                    "type": {
                        "module": "crm",
                        "code": "CONTACT",
                    },
                },
                {
                    "id": 3063,
                    "type": {
                        "module": "crm",
                        "code": "COMPANY",
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
                'booking.v1.waitlist.client.set',
                [
                    'waitListId' => 13,
                    'clients'    => [
                        [
                            'id'   => 2795,
                            'type' => [
                                'module' => 'crm',
                                'code'   => 'CONTACT',
                            ],
                        ],
                        [
                            'id'   => 3063,
                            'type' => [
                                'module' => 'crm',
                                'code'   => 'COMPANY',
                            ],
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result[0] === true) {
            echo 'Success';
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting waitlist clients: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.waitlist.client.set",
        {
            waitListId: 13,
            clients: [
                {
                    id: 2795,
                    type: {
                        module: "crm",
                        code: "CONTACT"
                    }
                },
                {
                    id: 3063,
                    type: {
                        module: "crm",
                        code: "COMPANY"
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
        'booking.v1.waitlist.client.set',
        [
            'waitListId' => 13,
            'clients' => [
                [
                    'id' => 2795,
                    'type' => [
                        'module' => 'crm',
                        'code' => 'CONTACT'
                    ]
                ],
                [
                    'id' => 3063,
                    'type' => [
                        'module' => 'crm',
                        'code' => 'COMPANY'
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
    res, err := client.Core().Call(ctx, "booking.v1.waitlist.client.set", b24.Params{
    	"waitListId": 13,
    	"clients": []b24.Params{
    		{
    			"id": 2795,
    			"type": b24.Params{
    				"module": "crm",
    				"code":   "CONTACT",
    			},
    		},
    		{
    			"id": 3063,
    			"type": b24.Params{
    				"module": "crm",
    				"code":   "COMPANY",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("booking.v1.waitlist.client.set: %w", err)
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
        "start": 1790294535,
        "finish": 1790294535.315704,
        "duration": 0.3157041072845459,
        "processing": 0,
        "date_start": "2026-09-25T03:02:15+03:00",
        "date_finish": "2026-09-25T03:02:15+03:00",
        "operating_reset_at": 1790295135,
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
    "error": "1040",
    "error_description": "Wait list not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Required fields: id` | The `id` field is not provided in a `clients` element. Specify the contact or company identifier ||
|| `0` | `Required fields: type` | The `type` object is not provided in a `clients` element ||
|| `0` | `Required fields: module` | The `module` field is not provided in the `type` object. For CRM, specify `crm` ||
|| `0` | `Required fields: code` | The `code` field is not provided in the `type` object. Specify `CONTACT` or `COMPANY` ||
|| `100` | `Could not find value for parameter {waitListId}` | The `waitListId` parameter is not provided. Specify the waitlist entry identifier ||
|| `100` | `Could not find value for parameter {clients}` | The `clients` array is not provided ||
|| `1025` | `Client type not found` | An unknown client type is passed. Check the `module` and `code` combination against the [booking.v1.clienttype.list](../../booking-v1-clienttype-list.md) method ||
|| `1040` | `Wait list not found` | The entry with the specified `waitListId` was not found. Check the identifier using the [booking.v1.waitlist.list](../booking-v1-waitlist-list.md) method ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-waitlist-client-unset.md)
- [{#T}](./booking-v1-waitlist-client-list.md)
