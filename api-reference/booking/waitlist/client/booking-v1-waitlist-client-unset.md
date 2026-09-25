# Remove Client Links from a Waitlist Entry booking.v1.waitlist.client.unset

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Removes all client links from the specified waitlist entry. The contacts and companies remain in the CRM.

{% note warning "Linked Deal" %}

If the entry no longer has clients but a deal is linked to it using the [booking.v1.waitlist.externalData.set](../external-data/booking-v1-waitlist-externaldata-set.md) method, calling this method links the contacts and company of that deal to the entry. As a result, a repeated call can populate the list again. Check the result using the [booking.v1.waitlist.client.list](./booking-v1-waitlist-client-list.md) method.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **waitListId***
[`integer`](../../../data-types.md) | Identifier of the waitlist entry.
Can be obtained using the methods [booking.v1.waitlist.add](../booking-v1-waitlist-add.md) and [booking.v1.waitlist.list](../booking-v1-waitlist-list.md) ||
|#

## Code Examples

The examples remove the client links of entry `13`. Replace the identifier with a value from your Bitrix24.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":13}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/booking.v1.waitlist.client.unset
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"waitListId":13,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/booking.v1.waitlist.client.unset
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
        method: 'booking.v1.waitlist.client.unset',
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
        console.info('Clients unset from waitlist:', result)
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
      async function unsetWaitlistClients() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'booking.v1.waitlist.client.unset',
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
          console.info('Clients unset from waitlist:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', unsetWaitlistClients)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.booking.v1.waitlist.client.unset(
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
                'booking.v1.waitlist.client.unset',
                [
                    'waitListId' => 13,
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
        echo 'Error unsetting waitlist client: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "booking.v1.waitlist.client.unset",
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
        'booking.v1.waitlist.client.unset',
        [
            'waitListId' => 13,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "booking.v1.waitlist.client.unset", b24.Params{
    	"waitListId": 13,
    })
    if err != nil {
    	return fmt.Errorf("booking.v1.waitlist.client.unset: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1790294539,
        "finish": 1790294539.731543,
        "duration": 0.7315430641174316,
        "processing": 0,
        "date_start": "2026-09-25T03:02:19+03:00",
        "date_finish": "2026-09-25T03:02:19+03:00",
        "operating_reset_at": 1790295139,
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

- [{#T}](./booking-v1-waitlist-client-set.md)
- [{#T}](./booking-v1-waitlist-client-list.md)