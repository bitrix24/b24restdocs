# How to Execute a Batch Request

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: any user

The `batch` method executes several Bitrix24 REST API requests in a single server call — either independent or linked, where the result of one request is passed to the next.

## When to Use Batch

The method is suitable for two tasks:

- **multiple independent calls in a single request** — when you need to execute a group of methods whose results do not depend on each other. Using one batch instead of several separate calls reduces the number of requests to the server.
- **linked calls with data passing** — when the result of one method must be passed into the parameters of the next. Requests are executed sequentially, so data from a previous call is available in subsequent calls.

Consider the following limitations:

- A single batch contains no more than 50 subqueries.
- Nesting is prohibited: you cannot call another `batch` inside `batch`.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **cmd***
[`array`](../../api-reference/data-types.md) | An array of subqueries. The `item` key is the subquery identifier, and the value is the called method with parameters as a string `method?parameter=value`. In the SDK, `item` can be specified as an object with fields `method` and `params` ||
|| **halt**
[`boolean`](../../api-reference/data-types.md) | Determines whether to interrupt the sequence of requests in case of an error. Accepts boolean values `true` and `false` or their numerical equivalents `1` and `0`. Defaults to `false` ||
|#

Subquery data is encoded differently depending on how the batch is transmitted. In a POST request body in `JSON` format, as in the examples below, values `cmd` are passed as plain strings without additional encoding. If the batch is passed via URL query parameters, the subquery data is [URL-encoded](./data-encoding.md), and since the entire batch itself becomes a parameter value, it undergoes double encoding.

{% note info %}

The number of requests in a batch is limited to 50. If this limit is exceeded, subqueries beyond the limit will end with an error `ERROR_BATCH_LENGTH_EXCEEDED`.

{% endnote %}

The array of requests can use numeric keys or be associative. In the parameters of each subsequent request, you can use data from previous requests in the following manner:

```php

$result[request_id][response_field]

```

where the request identifier is its key in the request array.

As of version **rest 24.0.0**, nesting is prohibited for the `batch` method: when calling the `batch` method, you cannot call another `batch` inside it. Such a subquery ends with an error `ERROR_BATCH_METHOD_NOT_ALLOWED`.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

### Independent Calls

A batch consisting of several different methods whose results do not depend on each other. Each subquery is executed separately, and no data is passed between them.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
            "halt": 0,
            "cmd": {
                "get_user": "user.current",
                "get_departments": "department.get",
                "get_app": "app.info"
            }
        }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/batch
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
            "halt": 0,
            "cmd": {
                "get_user": "user.current",
                "get_departments": "department.get",
                "get_app": "app.info"
            },
            "auth":"**put_access_token_here**"
        }' \
    https://**put_your_bitrix24_address**/rest/batch
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      // Named commands: each key becomes the identifier of its subrequest
      const response = await $b24.actions.v2.batch.make({
        calls: {
          get_user: { method: 'user.current', params: {} },
          get_departments: { method: 'department.get', params: {} },
          get_app: { method: 'app.info', params: {} }
        },
        options: {
          isHaltOnError: false, // analog of halt = 0: run every subrequest
          returnAjaxResult: true, // wrap each subrequest result in an AjaxResult
          requestId: Text.getUuidRfc4122()
        }
      })

      // isSuccess reflects the batch call as a whole
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        // getData() returns an object keyed by the command names above
        const result = response.getData()
        console.info(result.get_user.getData()?.result)
        console.info(result.get_departments.getData()?.result)
        console.info(result.get_app.getData()?.result)
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
      async function runBatch() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // Named commands: each key becomes the identifier of its subrequest
          const response = await $b24.actions.v2.batch.make({
            calls: {
              get_user: { method: 'user.current', params: {} },
              get_departments: { method: 'department.get', params: {} },
              get_app: { method: 'app.info', params: {} }
            },
            options: {
              isHaltOnError: false, // analog of halt = 0: run every subrequest
              returnAjaxResult: true, // wrap each subrequest result in an AjaxResult
              requestId: B24Js.Text.getUuidRfc4122()
            }
          })

          // isSuccess reflects the batch call as a whole
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          // getData() returns an object keyed by the command names above
          const result = response.getData()
          console.info(result.get_user.getData()?.result)
          console.info(result.get_departments.getData()?.result)
          console.info(result.get_app.getData()?.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', runBatch)
    </script>
    ```

- BX24.js

    ```js
    BX24.callBatch({
        get_user: ['user.current', {}],
        get_departments: ['department.get', {}],
        get_app: ['app.info', {}]
    }, function(result) {

        console.log('get_user result: ', result.get_user.data());
        console.log('get_departments result: ', result.get_departments.data());
        console.log('get_app result: ', result.get_app.data());
    });
    ```

    For more details, see the [callBatch method article in the BX24.JS SDK documentation](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-batch.md).

- PHP CRest

    ```php
    $result = \CRest::callBatch(
        // Commands
        [
            'get_user' => [
                'method' => 'user.current',
                'params' => []
            ],
            'get_departments' => [
                'method' => 'department.get',
                'params' => []
            ],
            'get_app' => [
                'method' => 'app.info',
                'params' => []
            ],
        ],
        // Halt
        false
    );

    echo "<pre>";
    var_dump($result);
    echo "</pre>";
    ```

{% endlist %}

### Linked Calls

Subqueries are executed sequentially, so the result of a previous request can be passed into the parameters of the next request using the `$result[request_id][response_field]` syntax. In the example below, the `department.get` method retrieves the `department` identifier from the result of `user.current`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
            "halt": 0,
            "cmd": {
                "get_user": "user.current",
                "get_department": "department.get?ID=$result[get_user][UF_DEPARTMENT][0]"
            }
        }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/batch
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
            "halt": 0,
            "cmd": {
                "get_user": "user.current",
                "get_department": "department.get?ID=$result[get_user][UF_DEPARTMENT][0]"
            },
            "auth":"**put_access_token_here**"
        }' \
    https://**put_your_bitrix24_address**/rest/batch
    ```

- BX24.js

    ```js
    BX24.callBatch({
        get_user: ['user.current', {}],
        get_department: {
            method: 'department.get',
            params: {
                ID: '$result[get_user][UF_DEPARTMENT][0]'
            }
        }
    }, function(result) {

        console.log('Raw result: ', result);
        console.log('get_user result: ', result.get_user.data());
        console.log('get_department result: ', result.get_department.data());
    });
    ```

    For more details, see the [callBatch method article in the BX24.JS SDK documentation](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-batch.md).

- PHP CRest

    ```php
    $result = \CRest::callBatch(
        // Commands
        [
            'get_user' => [
                'method' => 'user.current',
                'params' => []
            ],
            'get_department' => [
                'method' => 'department.get',
                'params' => [
                    "ID" => '$result[get_user][UF_DEPARTMENT][0]'
                ]
            ],
        ],
        // Halt
        false
    );

    echo "<pre>";
    var_dump($result);
    echo "</pre>";
    ```

{% endlist %}

{% note info %}

If an intermediate method is a list method, it returns an array of records; therefore, when accessing the result, specify the index of the required record. For example, in the `$result[user_by_name][0][ID]` syntax for the `user.search` method, the identifier of the first found user is retrieved.

{% endnote %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": {
            "get_user": {
                "ID": "1",
                "ACTIVE": true,
                "NAME": "John",
                "LAST_NAME": "Doe",
                "EMAIL": "my@example.com",
                "LAST_LOGIN": "2024-08-29T10:29:54+03:00",
                "DATE_REGISTER": "2023-08-24T03:00:00+03:00",
                "IS_ONLINE": "Y",
                "TIMESTAMP_X": "24.08.2023 13:19:39",
                "LAST_ACTIVITY_DATE": "2024-08-29 10:30:11",
                "PERSONAL_GENDER": "",
                "PERSONAL_BIRTHDAY": "",
                "UF_EMPLOYMENT_DATE": "",
                "UF_DEPARTMENT": [
                    1
                ]
            },
            "get_department": [
                {
                    "ID": "1",
                    "NAME": "DEMO",
                    "SORT": 500
                }
            ]
        },
        "result_error": [],
        "result_total": {
            "get_department": 1
        },
        "result_next": [],
        "result_time": {
            "get_user": {
                "start": 1724916859.46156,
                "finish": 1724916859.464775,
                "duration": 0.0032150745391845703,
                "processing": 0.003075838088989258,
                "date_start": "2024-08-29T10:34:19+03:00",
                "date_finish": "2024-08-29T10:34:19+03:00"
            },
            "get_department": {
                "start": 1724916859.464944,
                "finish": 1724916859.471518,
                "duration": 0.006574153900146484,
                "processing": 0.005941152572631836,
                "date_start": "2024-08-29T10:34:19+03:00",
                "date_finish": "2024-08-29T10:34:19+03:00"
            }
        }
    },
    "time": {
        "start": 1724916859.421475,
        "finish": 1724916859.471588,
        "duration": 0.05011296272277832,
        "processing": 0.010200977325439453,
        "date_start": "2024-08-29T10:34:19+03:00",
        "date_finish": "2024-08-29T10:34:19+03:00"
    }
}
```

## Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../api-reference/data-types.md) | The root response object containing the results of the called methods [(detailed description)](#result) ||
|| **time**
[`time`](../../api-reference/data-types.md) | Information about the total request execution time ||
|#

### Result Object {#result}

The keys in all object fields are the subquery identifiers from the `cmd` array.

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../api-reference/data-types.md) | Results of successfully executed subqueries. The value of each field contains the data returned by the corresponding method ||
|| **result_error**
[`object`](../../api-reference/data-types.md) | Subquery errors. The value of each field contains `error` — the error code and `error_description` — the error description. Empty if there are no errors ||
|| **result_total**
[`object`](../../api-reference/data-types.md) | Total number of records for list methods. The value of each field is the number of records found for the corresponding subquery ||
|| **result_next**
[`object`](../../api-reference/data-types.md) | The value of the `start` parameter to retrieve the next page of results for list methods. Present only for subqueries that have a next page ||
|| **result_time**
[`time`](../../api-reference/data-types.md) | Information about the execution time of each subquery ||
|#

## Error Handling

The `batch` method does not return a single error for the entire batch — the request itself completes with a **200** status. The result of each sub-request that ends in an error is placed in the `result_error` field under the key of that specific sub-request. Successfully executed sub-requests remain in the `result` field.

Error behavior depends on the `halt` parameter:

- `halt = 0` — all sub-requests in the batch are executed, and errors are collected in `result_error` for each problematic sub-request
- `halt = 1` — execution of the chain is interrupted at the first sub-request that returns an error; subsequent sub-requests are not executed

{% list tabs %}

- Error example (halt = 0)

    ```json
    {
        "result": {
            "result": [],
            "result_error": {
                "get_user": {
                    "error": "insufficient_scope",
                    "error_description": ""
                },
                "get_department": {
                    "error": "insufficient_scope",
                    "error_description": ""
                }
            },
            "result_total": [],
            "result_next": [],
            "result_time": []
        },
        "time": {
            "start": 1724916638.077564,
            "finish": 1724916638.132399,
            "duration": 0.05483508110046387,
            "processing": 0.0017969608306884766,
            "date_start": "2024-08-29T10:30:38+03:00",
            "date_finish": "2024-08-29T10:30:38+03:00"
        }
    }
    ```

- Error example (halt = 1)

    ```json
    {
        "result": {
            "result": [],
            "result_error": {
                "get_user": {
                    "error": "insufficient_scope",
                    "error_description": ""
                }
            },
            "result_total": [],
            "result_next": [],
            "result_time": []
        },
        "time": {
            "start": 1724916725.460891,
            "finish": 1724916725.851307,
            "duration": 0.39041590690612793,
            "processing": 0.0005991458892822266,
            "date_start": "2024-08-29T10:32:05+03:00",
            "date_finish": "2024-08-29T10:32:05+03:00"
        }
    }
    ```

{% endlist %}

Each item in the `result_error` field contains information about the sub-request error:

{% include notitle [Error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `405` | `ERROR_BATCH_METHOD_NOT_ALLOWED` | Method is not allowed for batch usage | The method cannot be called inside `batch`: this is a file upload or download or a nested `batch` ||
|| `400` | `ERROR_BATCH_LENGTH_EXCEEDED` | Max batch length exceeded | More than 50 subqueries were passed in the batch ||
|#

When designing a command chain, do not neglect the `halt` key — with a value of `1`, it will interrupt the execution of the chain if one request in the chain returns an error.

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./data-encoding.md)
- [{#T}](./list-methods-pecularities.md)
- [{#T}](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-batch.md)
