# Get a List of Offline Events event.offline.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The `event.offline.list` method reads the [offline event](./offline-events.md) queue of the application that called it. Unlike [event.offline.get](./event-offline-get.md), it does not reserve records and does not return `process_id`. To confirm records or mark them as erroneous, retrieve them with the `event.offline.get` method with `clear=0`.

The method works only in the context of authorizing the [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`object`](../data-types.md) | Record filter. Without a filter, the method returns all records. You can filter by the fields: `ID`, `TIMESTAMP_X`, `EVENT_NAME`, `MESSAGE_ID`, `PROCESS_ID`, `ERROR`.

`TIMESTAMP_X` is passed in ISO 8601 format, `ERROR` — as `0` or `1`.

The operation is specified before the field name: `=`, `>`, `<`, `>=`, `<=`, `@` — the value is in the array, `%` — substring. Without an operation, an exact match is used. Example: `{">ID": 100, "=EVENT_NAME": "ONCRMLEADADD"}`. Negation `!` is not supported: the method returns the `ERROR_ARGUMENT` error ||
|| **order**
[`object`](../data-types.md) | Record sorting by the same fields as in the filter, in the form `{"field": "ASC"}` or `{"field": "DESC"}`. Default is `{"ID": "ASC"}` ||
|| **start**
[`integer`](../data-types.md) | This parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|| **auth_connector**
[`string`](../data-types.md) | Source key. The queue of offline events is divided by sources. Pass the same `auth_connector` value as when subscribing with the [event.bind](./event-bind.md) method; otherwise, the method will return only events without a source. The parameter is not available on all plans: check its availability using the [feature.get](../common/system/feature-get.md) method with the code `rest_auth_connector`, otherwise the method returns the `WRONG_LICENSE` error ||
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
        "filter": {
            "ERROR": 0
        },
        "order": {
            "ID": "DESC"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/event.offline.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each OfflineEventItem returned in result[]
    type OfflineEventItem = {
      ID: string
      TIMESTAMP_X: ISODate
      EVENT_NAME: string
      EVENT_DATA: unknown
      EVENT_ADDITIONAL: unknown
      MESSAGE_ID: string
      PROCESS_ID: string
      ERROR: string
    }

    try {
      // event.offline.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers ignore `order` (they always sort by ID ASC and log a warning)
      // — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<OfflineEventItem[]>({
        method: 'event.offline.list',
        params: {
          filter: {
            ERROR: 0,
          },
          order: {
            ID: 'DESC',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Offline events:', result.length, result)
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
      async function fetchOfflineEvents() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // event.offline.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers ignore `order` (they always sort by ID ASC and log a warning)
          // — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'event.offline.list',
            params: {
              filter: {
                ERROR: 0,
              },
              order: {
                ID: 'DESC',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Offline events:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchOfflineEvents)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.event.offline.list(
            filter={
                "ERROR": 0,
            },
            order={
                "ID": "DESC",
            },
            start=0,
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

    Example `as_list`

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.event.offline.list(
            filter={
                "ERROR": 0,
            },
            order={
                "ID": "DESC",
            },
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
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

    Example `as_list_fast`

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.event.offline.list(
            filter={
                "ERROR": 0,
            },
        ).as_list_fast(descending=True).response
        result = bitrix_response.result
        for item in result:
            print(item)
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
                'event.offline.list',
                [
                    'filter' => [
                        'ERROR' => 0,
                    ],
                    'order' => [
                        'ID' => 'DESC',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching offline events: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "event.offline.list",
        {
            "filter": {
                "ERROR": 0
            },
            "order": {
                "ID": "DESC"
            }
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
        'event.offline.list',
        [
            'filter' => [
                'ERROR' => 0
            ],
            'order' => [
                'ID' => 'DESC'
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
    res, err := client.Core().Call(ctx, "event.offline.list", b24.Params{
    	"filter": b24.Params{
    		"ERROR": 0,
    	},
    	"order": b24.Params{
    		"ID": "DESC",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("event.offline.list: %w", err)
    }

    var items []struct {
    	ID              b24.ID          `json:"ID"`
    	TimestampX      string          `json:"TIMESTAMP_X"`
    	EventName       string          `json:"EVENT_NAME"`
    	EventData       json.RawMessage `json:"EVENT_DATA"`
    	EventAdditional json.RawMessage `json:"EVENT_ADDITIONAL"`
    	MessageID       string          `json:"MESSAGE_ID"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.TimestampX)
    }

    // Total and Next are filled in by list methods; for a full
    // list traversal, use client.Core().Pages and Scan.
    if res.Total != nil {
    	fmt.Println("total:", *res.Total)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "2",
            "TIMESTAMP_X": "2024-07-18T12:32:31+02:00",
            "EVENT_NAME": "ONCRMCOMPANYADD",
            "EVENT_DATA": {
                "FIELDS": {
                    "ID": "45"
                }
            },
            "EVENT_ADDITIONAL": {
                "user_id": "1"
            },
            "MESSAGE_ID": "4f2a9c1e7b3d5a6f8e0c2b4d6a8f1e3c",
            "PROCESS_ID": "",
            "ERROR": "0"
        },
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
                "user_id": 0
            },
            "MESSAGE_ID": "b0c7f7c2a3f1e4d5c6b7a8f9e0d1c2b3",
            "PROCESS_ID": "",
            "ERROR": "0"
        }
    ],
    "total": 2,
    "time": {
        "start": 1721299537.90267,
        "finish": 1721299538.02201,
        "duration": 0.11934018135070801,
        "processing": 0.0029511451721191406,
        "date_start": "2024-07-18T12:45:37+02:00",
        "date_finish": "2024-07-18T12:45:38+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Queue records [(detailed description)](#event). If there are no matching records, an empty array is returned ||
|| **total**
[`integer`](../data-types.md) | The total number of records found ||
|| **next**
[`integer`](../data-types.md) | The `start` value for the next page. Returned if more records are found than fit on the current page ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

#### List Element {#event}

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
|| **PROCESS_ID**
[`string`](../data-types.md) | Identifier of the batch that reserved the record via the [event.offline.get](./event-offline-get.md) method with `clear=0`. An empty string if the record is not reserved ||
|| **ERROR**
[`string`](../data-types.md) | `1` — the record is marked as erroneous by the [event.offline.error](./event-offline-error.md) method, `0` — it is not ||
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
|| `400` | `ERROR_ARGUMENT` | Filter field not allowed: … | A field not from the list was passed in `filter` ||
|| `400` | `ERROR_ARGUMENT` | Filter operation not allowed: … | An unsupported operation was passed in `filter`, for example `!` ||
|| `400` | `ERROR_ARGUMENT` | The filter is not an array. | The `filter` parameter was not passed as an object ||
|| `400` | `ERROR_ARGUMENT` | The order is not an array. | The `order` parameter was not passed as an object ||
|| `400` | `ERROR_ARGUMENT` | Order field not allowed: … | A field not from the list was passed in `order` ||
|| `400` | `ERROR_ARGUMENT` | ```Order direction should be one of {ASC|DESC}``` | A direction other than `ASC` or `DESC` was passed in `order` ||
|| `403` | `ACCESS_DENIED` | Access denied! | The method was called by a non-administrator ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | The method was called outside an application, for example, through a webhook ||
|| `403` | `WRONG_LICENSE` | This feature is not enabled for the current license: auth_connector | `auth_connector` was passed, but the plan does not support source keys ||
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
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
