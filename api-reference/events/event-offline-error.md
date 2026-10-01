# Register Offline Event Queue Processing Errors event.offline.error

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The `event.offline.error` method marks records of a reserved [offline event](./offline-events.md) batch as erroneous. Such records are released from the reservation and are no longer included in the regular output of [event.offline.get](./event-offline-get.md). You can retrieve them with the `event.offline.get` method with the `error=1` parameter.

The batch reservation mode is not available on all plans. Check its availability using the [feature.get](../common/system/feature-get.md) method with the code `rest_offline_extended`.

The method works only in the context of authorizing the [application](../../settings/app-installation/index.md). Permissions for event methods are described in the [Access Permissions](./index.md#access) section.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **process_id***
[`string`](../data-types.md) | Identifier of the reserved event batch. It is returned by the [event.offline.get](./event-offline-get.md) method when called with the `clear=0` parameter ||
|| **message_id***
[`array`](../data-types.md) | Array of `MESSAGE_ID` field values from the `event.offline.get` response — the keys of the records to mark as erroneous. A key is a string of 32 characters. The method does not check the length: with an invalid key, the record is not found and no error is returned. The method does not change records that are already marked as erroneous. If you pass an empty array, the method returns `true` and changes nothing ||
|| **auth_connector**
[`string`](../data-types.md) | Source key. Pass the same value with which the batch was retrieved by the `event.offline.get` method, otherwise the method does not find the records. The parameter is not available on all plans: check it using the [feature.get](../common/system/feature-get.md) method with the code `rest_auth_connector`, otherwise the method returns the `WRONG_LICENSE` error ||
|#

{% note warning %}

The method marks as erroneous only the records of the batch with the passed `process_id`. Records with the same `MESSAGE_ID` values that are not yet processed and do not belong to this batch are deleted from the queue. Therefore, with someone else's or an invalid `process_id`, the method marks nothing, deletes the records, and returns `true`. With an empty string in `process_id`, the method marks as erroneous the records that are not yet reserved in a batch and deletes the records with the same keys from other batches.

{% endnote %}

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
        "message_id": ["b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3"],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/event.offline.error
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
        method: 'event.offline.error',
        params: {
          process_id: 'yh3gu929sf0d32lsfysqas2y1hlpp09q',
          message_id: ['b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3'],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Offline error registered:', result)
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
      async function registerOfflineError() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.offline.error',
            params: {
              process_id: 'yh3gu929sf0d32lsfysqas2y1hlpp09q',
              message_id: ['b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Offline error registered:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', registerOfflineError)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.event.offline.error(
            process_id="yh3gu929sf0d32lsfysqas2y1hlpp09q",
            message_id=[
                "b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3",
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
                'event.offline.error',
                [
                    'process_id' => 'yh3gu929sf0d32lsfysqas2y1hlpp09q',
                    'message_id' => ['b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3'],
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
        echo 'Error handling offline event: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "event.offline.error",
        {
            "process_id": "yh3gu929sf0d32lsfysqas2y1hlpp09q",
            "message_id": ["b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3"]
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
        'event.offline.error',
        [
            'process_id' => 'yh3gu929sf0d32lsfysqas2y1hlpp09q',
            'message_id' => ['b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "event.offline.error", b24.Params{
    	"process_id": "yh3gu929sf0d32lsfysqas2y1hlpp09q",
    	"message_id": []string{"b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3"},
    })
    if err != nil {
    	return fmt.Errorf("event.offline.error: %w", err)
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
        "start": 1721300231.498173,
        "finish": 1721300231.596196,
        "duration": 0.0980229377746582,
        "processing": 0.0019490718841552734,
        "date_start": "2024-07-18T12:57:11+02:00",
        "date_finish": "2024-07-18T12:57:11+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Always `true` if the method did not return an error. The method does not return the number of marked records ||
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
|| `400` | `ERROR_ARGUMENT` | Value must be array of MESSAGE_ID values | The `message_id` parameter is not passed or is not an array ||
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
- [{#T}](./event-offline-clear.md)
- [{#T}](./on-offline-event.md)
