# Get Clients of a Waitlist Entry booking.v1.waitlist.client.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `booking.v1.waitlist.client.list` returns a list of clients for the specified entry in the waitlist.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **waitListId***
[`integer`](../../../data-types.md) | Identifier of the entry in the waitlist.
Can be obtained using the methods [booking.v1.waitlist.add](../booking-v1-waitlist-add.md) and [booking.v1.waitlist.list](../booking-v1-waitlist-list.md) ||
|#

## Code Examples

The examples retrieve the clients of entry `13`. Replace the identifier with a value from your Bitrix24.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":13}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.waitlist.client.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":13,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.waitlist.client.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type WaitListClientListResult = {
      waitListClient: Array<{
        id: number
        type: {
          code: string
          module: string
        }
      }>
    }

    try {
      const response = await $b24.actions.v2.call.make<WaitListClientListResult>({
        method: 'booking.v1.waitlist.client.list',
        params: {
          waitListId: 13,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Clients:', result.waitListClient, 'Count:', result.waitListClient.length)
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
      async function listWaitListClients() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'booking.v1.waitlist.client.list',
            params: {
              waitListId: 13,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Clients:', result.waitListClient, 'Count:', result.waitListClient.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listWaitListClients)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.booking.v1.waitlist.client.list(
            wait_list_id=13,
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
                'booking.v1.waitlist.client.list',
                [
                    'waitListId' => 13,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Clients: ' . print_r($result['waitListClient'], true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing waitlist clients: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.waitlist.client.list",
        {
            waitListId: 13,
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
        'booking.v1.waitlist.client.list',
        [
            'waitListId' => 13
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "booking.v1.waitlist.client.list", b24.Params{
    	"waitListId": 13,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("booking.v1.waitlist.client.list: %w", err)
    }

    // The method wraps the response in an object with the "waitListClient" key.
    raw, ok := b24.Unwrap(res.Result, "waitListClient")
    if !ok {
    	return fmt.Errorf("no waitListClient key in the response")
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
        "waitListClient": [
            {
                "id": 2795,
                "type": {
                    "code": "CONTACT",
                    "module": "crm"
                }
            },
            {
                "id": 3063,
                "type": {
                    "code": "COMPANY",
                    "module": "crm"
                }
            }
        ]
    },
    "total": 0,
    "time": {
        "start": 1790294537,
        "finish": 1790294537.508353,
        "duration": 0.5083529949188232,
        "processing": 0,
        "date_start": "2026-09-25T03:02:17+03:00",
        "date_finish": "2026-09-25T03:02:17+03:00",
        "operating_reset_at": 1790295137,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the `waitListClient` array. [Object Structure](#result) ||
|| **total**
[`integer`](../../../data-types.md) | Internal field, returns `0`. To find out the number of clients, count the elements of `result.waitListClient` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **waitListClient**
[`array`](../../../data-types.md) | Array of objects with the `id` and `type` fields. If the entry has no clients, an empty array `[]` is returned. [Element Structure](#waitListClient) ||
|#

#### Client {#waitListClient}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of a CRM contact or company. To retrieve its data using [crm.item.get](../../../crm/universal/crm-item-get.md), pass `entityTypeId: 3` for a contact or `entityTypeId: 4` for a company ||
|| **type**
[`object`](../../../data-types.md) | Client type. [Object Structure](#client-type) ||
|#

#### type Object {#client-type}

#|
|| **Name**
`type` | **Description** ||
|| **module**
[`string`](../../../data-types.md) | Client module. For contacts and companies, `crm` ||
|| **code**
[`string`](../../../data-types.md) | Client type code:
- `CONTACT` — [CRM contact](../../../crm/contacts/index.md)
- `COMPANY` — [CRM company](../../../crm/companies/index.md)

The available client types are returned by the [booking.v1.clienttype.list](../../booking-v1-clienttype-list.md) method ||
|#

## Error Handling

HTTP status: **400**

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
|| `100` | `Could not find value for parameter {waitListId}` | The `waitListId` parameter is not provided. Specify the waitlist entry identifier ||
|| `1040` | `Wait list not found` | The entry with the specified `waitListId` was not found. Check the identifier using the [booking.v1.waitlist.list](../booking-v1-waitlist-list.md) method ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./booking-v1-waitlist-client-unset.md)
- [{#T}](./booking-v1-waitlist-client-set.md)
