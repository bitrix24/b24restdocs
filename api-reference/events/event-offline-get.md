# Retrieve a List of Offline Events With Cleanup event.offline.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The `event.offline.get` method returns to the application the first records from the [offline event](./offline-events.md) queue that match the filter. The method has two modes:

- `clear=1`, the default — returns the records and immediately deletes them from the queue
- `clear=0` — reserves the records as a batch and returns its `process_id`. Confirm processing of the batch with the [event.offline.clear](./event-offline-clear.md) method, and mark errors with the [event.offline.error](./event-offline-error.md) method

The batch reservation mode is not available on all plans. Check its availability using the [feature.get](../common/system/feature-get.md) method with the code `rest_offline_extended`.

The method works only in the context of authorizing the [application](../../settings/app-installation/index.md). Permissions for event methods are described in the [Access Permissions](./index.md#access) section.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`object`](../data-types.md) | Record filter. By default, the method returns all records. You can filter by the fields: `ID`, `TIMESTAMP_X`, `EVENT_NAME`, `MESSAGE_ID`.

Specify the operation before the field name: `=`, `>`, `<`, `>=`, `<=`. Without an operation, an exact match is used. Negation `!` is not supported, such a filter returns the `ERROR_ARGUMENT` error. Example: `{">ID": 100, "=EVENT_NAME": "ONCRMLEADADD"}`.

The value is a string or a number, arrays are not supported. Pass `TIMESTAMP_X` in ISO 8601 format, for example `2024-07-18T12:32:31+02:00` ||
|| **order**
[`object`](../data-types.md) | Record sorting by the same fields as in the filter, in the form `{"field": "ASC"}` or `{"field": "DESC"}`. Default is `{"TIMESTAMP_X": "ASC"}` ||
|| **limit**
[`integer`](../data-types.md) | Number of records to select, an integer greater than `0`. Default is 50 ||
|| **clear**
[`integer`](../data-types.md) | Whether to delete the selected records: `1` — delete immediately, `0` — reserve as a batch. Default is `1`. The value `0` without the `rest_offline_extended` mode returns the `WRONG_LICENSE` error ||
|| **process_id**
[`string`](../data-types.md) | Identifier of a previously reserved batch. Pass it to retrieve the batch records that have not been confirmed yet. With `clear=1`, the method deletes the entire batch, including records that were not included in the response due to `limit`. To keep the records, pass `clear=0` ||
|| **auth_connector**
[`string`](../data-types.md) | Source key. Pass the same `auth_connector` value as when subscribing with the [event.bind](./event-bind.md) method, otherwise the method returns only events without a source. The parameter is not available on all plans: check its availability using the [feature.get](../common/system/feature-get.md) method with the code `rest_auth_connector`, otherwise the method returns the `WRONG_LICENSE` error ||
|| **error**
[`integer`](../data-types.md) | `1` — return only records marked as erroneous by the [event.offline.error](./event-offline-error.md) method, `0` — only records without such a mark. Default is `0` ||
|#

{% note info %}

The method can be called in parallel: each request receives its own set of records that does not overlap with the others. Keep in mind the [request rate limits](../../limits.md).

{% endnote %}

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"=MESSAGE_ID":"b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3","=EVENT_NAME":"ONCRMLEADADD",">=ID":1},"auth_connector":"BxTest","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/event.offline.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type OfflineGetResult = {
      process_id: string | null,
      events: {
        ID: string,
        TIMESTAMP_X: ISODate,
        EVENT_NAME: string,
        EVENT_DATA: unknown,
        EVENT_ADDITIONAL: unknown,
        MESSAGE_ID: string,
      }[],
    }

    try {
      const response = await $b24.actions.v2.call.make<OfflineGetResult>({
        method: 'event.offline.get',
        params: {
          filter: {
            '=MESSAGE_ID': 'b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3',
            '=EVENT_NAME': 'ONCRMLEADADD',
            '>=ID': 1,
          },
          auth_connector: 'BxTest',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Offline events:', result.events, 'Process ID:', result.process_id)
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
      async function getOfflineEvents() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'event.offline.get',
            params: {
              filter: {
                '=MESSAGE_ID': 'b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3',
                '=EVENT_NAME': 'ONCRMLEADADD',
                '>=ID': 1,
              },
              auth_connector: 'BxTest',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Offline events:', result.events, 'Process ID:', result.process_id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOfflineEvents)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.event.offline.get(
            filter={
                "=MESSAGE_ID": "b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3",
                "=EVENT_NAME": "ONCRMLEADADD",
                ">=ID": 1,
            },
            auth_connector="BxTest",
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
                'event.offline.get',
                [
                    'filter' => [
                        '=MESSAGE_ID' => 'b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3',
                        '=EVENT_NAME' => 'ONCRMLEADADD',
                        '>=ID' => 1
                    ],
                    'auth_connector' => 'BxTest'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "event.offline.get",
        {
            "filter": {
                "=MESSAGE_ID": "b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3",
                "=EVENT_NAME": "ONCRMLEADADD",
                ">=ID": 1
            },
            "auth_connector": "BxTest"
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
        'event.offline.get',
        [
            'filter' => [
                '=MESSAGE_ID' => 'b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3',
                '=EVENT_NAME' => 'ONCRMLEADADD',
                '>=ID' => 1
            ],
            'auth_connector' => 'BxTest'
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
        "process_id": null,
        "events": [
            {
                "ID": "1",
                "TIMESTAMP_X": "2024-07-18T12:32:31+02:00",
                "EVENT_NAME": "ONCRMLEADADD",
                "EVENT_DATA": {
                    "FIELDS": {
                        "ID": "123"
                    }
                },
                "EVENT_ADDITIONAL": {
                    "user_id": "1"
                },
                "MESSAGE_ID": "b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3"
            }
        ]
    },
    "time": {
        "start": 1721299720.388504,
        "finish": 1721299720.509809,
        "duration": 0.12130498886108398,
        "processing": 0.008239030838012695,
        "date_start": "2024-07-18T12:48:40+02:00",
        "date_finish": "2024-07-18T12:48:40+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Batch identifier and the selected queue records [(detailed description)](#result) ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **process_id**
[`string`](../data-types.md) | Batch identifier. Populated if the method is called with `clear=0` or with the `process_id` parameter, otherwise `null`. Pass it to [event.offline.clear](./event-offline-clear.md) or [event.offline.error](./event-offline-error.md) ||
|| **events**
[`array`](../data-types.md) | Queue records [(detailed description)](#event). If there are no matching records, an empty array is returned ||
|#

#### Element of the Events Array {#event}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Identifier of the record in the queue ||
|| **TIMESTAMP_X**
[`datetime`](../data-types.md) | Time the event was written to the queue or last repeated ||
|| **EVENT_NAME**
[`string`](../data-types.md) | Event code, for example `ONCRMLEADADD` ||
|| **EVENT_DATA**
[`object`](../data-types.md) or [`boolean`](../data-types.md) | Event data — the same as passed to the handler of an online event, for example `FIELDS.ID`. If the event has no data, the field is empty: `false` or `null` ||
|| **EVENT_ADDITIONAL**
[`object`](../data-types.md) | Event authorization data. The `user_id` field contains the identifier of the user who performed the action. If the action was performed without a user, for example by an agent, `user_id` is `0` ||
|| **MESSAGE_ID**
[`string`](../data-types.md) | Record key. A repeated event with the same data updates the unreserved record instead of adding a new one. If the record is already reserved by a batch, the repeat creates a new record. Pass the value in the `message_id` parameter of the [event.offline.clear](./event-offline-clear.md) and [event.offline.error](./event-offline-error.md) methods ||
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
|| `400` | `ERROR_ARGUMENT` | Value must be positive integer | The `limit` parameter is less than `1` ||
|| `400` | `ERROR_ARGUMENT` | Filter field not allowed: … | A field not from the list was passed in `filter` ||
|| `400` | `ERROR_ARGUMENT` | Filter operation not allowed: … | An unsupported operation was passed in `filter`, for example `!` ||
|| `400` | `ERROR_ARGUMENT` | The value of an argument '…' has an invalid type | An array was passed in `filter` instead of a string or a number ||
|| `400` | `ERROR_ARGUMENT` | The filter is not an array. | The `filter` parameter was not passed as an object ||
|| `400` | `ERROR_ARGUMENT` | The order is not an array. | The `order` parameter was not passed as an object ||
|| `400` | `ERROR_ARGUMENT` | Order field not allowed: … | A field not from the list was passed in `order` ||
|| `400` | `ERROR_ARGUMENT` | ```Order direction should be one of {ASC|DESC}``` | A direction other than `ASC` or `DESC` was passed in `order` ||
|| `403` | `ACCESS_DENIED` | Access denied! | The method was called by a non-administrator ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | The method was called outside an application, for example, through a webhook ||
|| `403` | `WRONG_LICENSE` | This feature is not enabled for the current license: auth_connector | `auth_connector` was passed, but the plan does not support source keys ||
|| `403` | `WRONG_LICENSE` | This feature is not enabled for the current license: extended offline events handling | The method was called with `clear=0`, but the `rest_offline_extended` mode is not available ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./events.md)
- [{#T}](./event-bind.md)
- [{#T}](./test-handler.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
