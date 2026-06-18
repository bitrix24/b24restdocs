# Get information about the current workday timeman.status

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.status` retrieves information about the current workday.

## Method parameters

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID**
[`integer`](../../data-types.md) | User identifier.

By default — the identifier of the current user ||
|#

## Code examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":503}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.status
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":503,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.status
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TimemanStatusResult = {
      STATUS: string
      TIME_START: ISODate | null
      TIME_FINISH: ISODate | null
      DURATION: string
      TIME_LEAKS: string
      ACTIVE: boolean
      IP_OPEN: string
      IP_CLOSE: string | null
      LAT_OPEN: number
      LON_OPEN: number
      LAT_CLOSE: number
      LON_CLOSE: number
      TZ_OFFSET: number
      TIME_FINISH_DEFAULT: ISODate | null
    }

    try {
      const response = await $b24.actions.v2.call.make<TimemanStatusResult>({
        method: 'timeman.status',
        params: {
          USER_ID: 503,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Status:', result.STATUS, 'Started:', result.TIME_START)
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
      async function getTimemanStatus() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'timeman.status',
            params: {
              USER_ID: 503,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Status:', result.STATUS, 'Started:', result.TIME_START)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTimemanStatus)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.status',
                [
                    'USER_ID' => 503
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling timeman.status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'timeman.status',
        {
            'USER_ID' : 503
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.status',
        [
            'USER_ID' => 503
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response handling

HTTP status: **200**

```json
{
    "result": {
        "STATUS": "EXPIRED",
        "TIME_START": "2025-03-27T08:00:01+02:00",
        "TIME_FINISH": null,
        "DURATION": "00:00:00",
        "TIME_LEAKS": "00:17:53",
        "ACTIVE": false,
        "IP_OPEN": "",
        "IP_CLOSE": "",
        "LAT_OPEN": 53.548841000000003,
        "LON_OPEN": 9.9872739999999993,
        "LAT_CLOSE": 0,
        "LON_CLOSE": 0,
        "TZ_OFFSET": 7200,
        "TIME_FINISH_DEFAULT": "2025-03-27T17:00:00+02:00"
    },
    "time": {
        "start": 1743057653.725821,
        "finish": 1743057654.0894129,
        "duration": 0.36359190940856934,
        "processing": 0.3278491497039795,
        "date_start": "2025-03-27T09:40:53+03:00",
        "date_finish": "2025-03-27T09:40:54+03:00",
        "operating_reset_at": 1743058253,
        "operating": 0.32782983779907227
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response.

Contains an object with the description of the workday ||
|| **STATUS**
 [`string`](../../data-types.md) | Status of the current workday.
 
 Possible values:
- `OPENED` — opened
- `CLOSED` — closed
- `PAUSED` — paused
- `EXPIRED` — expired, meaning opened before the start of the current calendar day and not closed ||
|| **TIME_START**
[`datetime`](../../data-types.md) | Date and time when the workday starts.

The time zone corresponds to the time zone at the start of the workday ||
|| **TIME_FINISH**
[`datetime`](../../data-types.md) | Date and time when the workday ends.

Returns `null` for an unfinished workday ||
|| **DURATION**
[`string`](../../data-types.md) | Duration of the workday in the format `HH:MM:SS`.

Returns `00:00:00` for an unfinished workday ||
|| **TIME_LEAKS**
[`string`](../../data-types.md) | Total duration of breaks during the day in the format `HH:MM:SS` ||
|| **ACTIVE**
[`boolean`](../../data-types.md) | Confirmation of the workday.

A value of `false` means that the change to the workday is awaiting confirmation from the supervisor ||
|| **IP_OPEN**
[`string`](../../data-types.md) | IP address from which the workday started ||
|| **IP_CLOSE**
[`string`](../../data-types.md) | IP address from which the workday ended.

Returns `null` for an unfinished workday ||
|| **LAT_OPEN**
[`double`](../../data-types.md) | Geographical latitude of the point where the workday started ||
|| **LON_OPEN**
[`double`](../../data-types.md) | Geographical longitude of the point where the workday started ||
|| **LAT_CLOSE**
[`double`](../../data-types.md) | Geographical latitude of the point where the workday ended ||
|| **LON_CLOSE**
[`double`](../../data-types.md) | Geographical longitude of the point where the workday ended ||
|| **TZ_OFFSET**
[`integer`](../../data-types.md) | Time zone offset of the employee where the workday started.

The end time of the workday is adjusted to the time zone at the start of the day ||
|| **TIME_FINISH_DEFAULT**
[`datetime`](../../data-types.md) | Recommended value for the end of the day, which can be displayed to the user as a default value.

Displayed only for workdays with the status expired `EXPIRED` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the time taken to process the request ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error":"",
    "error_description":"User not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** | **Value** ||
|| empty string | User not found | User with the specified `USER_ID` not found ||
|#

## Continue exploring 

- [{#T}](./index.md)
- [{#T}](./timeman-open.md)
- [{#T}](./timeman-pause.md)
- [{#T}](./timeman-close.md)
- [{#T}](./timeman-settings.md)
