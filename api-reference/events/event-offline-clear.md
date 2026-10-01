# Clear Offline Event Queue event.offline.clear

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The `event.offline.clear` method deletes from the queue the records of a batch retrieved by the [event.offline.get](./event-offline-get.md) method with `clear=0`. This is how the application confirms that it has processed these records. Mark the records that could not be processed with the [event.offline.error](./event-offline-error.md) method. The batch workflow is described in the article [{#T}](./offline-events.md).

The batch reservation mode is not available on all plans. Check its availability using the [feature.get](../common/system/feature-get.md) method with the code `rest_offline_extended`.

The method works only in the context of authorizing the [application](../../settings/app-installation/index.md). Permissions for event methods are described in the [Access Permissions](./index.md#access) section.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **process_id***
[`string`](../data-types.md) | Identifier of the reserved event batch. It is returned by the [event.offline.get](./event-offline-get.md) method when called with the `clear=0` parameter. The value is also available in the `PROCESS_ID` field of the records returned by the [event.offline.list](./event-offline-list.md) method. Do not pass an empty string: the method does not return an error and deletes from the queue all of the application's records outside batches, including those marked as erroneous ||
|| **id**
[`array`](../data-types.md) | Array of `ID` field values of the records to delete — integers greater than `0`. Ignored if the `message_id` parameter is passed. By default and when the array is empty, all records of the `process_id` batch are deleted ||
|| **message_id**
[`array`](../data-types.md) | Array of `MESSAGE_ID` field values of the records to delete — strings of 32 characters. By default and when the array is empty, all records of the `process_id` batch are deleted ||
|| **auth_connector**
[`string`](../data-types.md) | Source key. Pass the same value with which the batch was retrieved by the `event.offline.get` method, otherwise the method deletes nothing. The parameter is not available on all plans: check it using the [feature.get](../common/system/feature-get.md) method with the code `rest_auth_connector`, otherwise the method returns the `WRONG_LICENSE` error ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "process_id": "yh3gu929sf0d32lsfysqas2y1hlpp09q",
        "id": [2],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/event.offline.clear
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
        method: 'event.offline.clear',
        params: {
          process_id: 'yh3gu929sf0d32lsfysqas2y1hlpp09q',
          id: [2],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Clear result:', result)
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
      async function clearOfflineEvents() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.offline.clear',
            params: {
              process_id: 'yh3gu929sf0d32lsfysqas2y1hlpp09q',
              id: [2],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Clear result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', clearOfflineEvents)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.event.offline.clear(
            process_id="yh3gu929sf0d32lsfysqas2y1hlpp09q",
            bitrix_id=[2],
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
                'event.offline.clear',
                [
                    'process_id' => 'yh3gu929sf0d32lsfysqas2y1hlpp09q',
                    'id'        => [2],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error clearing offline event: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "event.offline.clear",
        {
            "process_id": "yh3gu929sf0d32lsfysqas2y1hlpp09q",
            "id": [2]
        },
        function(result)
        {
            if(result.error())
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
        'event.offline.clear',
        [
            'process_id' => 'yh3gu929sf0d32lsfysqas2y1hlpp09q',
            'id' => [2]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "event.offline.clear", b24.Params{
    	"process_id": "yh3gu929sf0d32lsfysqas2y1hlpp09q",
    	"id":         []int{2},
    })
    if err != nil {
    	return fmt.Errorf("event.offline.clear: %w", err)
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
        "start": 1721300421.210707,
        "finish": 1721300421.331026,
        "duration": 0.12031912803649902,
        "processing": 0.0022459030151367188,
        "date_start": "2024-07-18T13:00:21+02:00",
        "date_finish": "2024-07-18T13:00:21+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Always `true` if the method did not return an error. The method does not return the number of deleted records: the response is `true` even if no records are found for the passed values ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [Error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_ARGUMENT` | Argument 'PROCESS_ID' is null or empty | The `process_id` parameter is not passed ||
|| `400` | `ERROR_ARGUMENT` | Value must be array of integers | The `id` parameter is not an array or contains a value less than `1` ||
|| `400` | `ERROR_ARGUMENT` | Value must be array of MESSAGE_ID values | The `message_id` parameter is not an array or contains a string that is not 32 characters long ||
|| `403` | `ACCESS_DENIED` | Access denied! | The method was called by a non-administrator ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | The method was called outside an application, for example, through a webhook ||
|| `403` | `WRONG_LICENSE` | This feature is not enabled for the current license: auth_connector | `auth_connector` is passed, but the plan does not support source keys ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./events.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
