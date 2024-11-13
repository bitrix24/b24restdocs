# How to Execute Batch Requests

> Who can execute the method: any user

This method is used to send multiple requests in succession, as well as related requests.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **halt**
[`boolean`](../data-types.md) | Determines whether to stop the sequence of requests in case of an error. Defaults to `false` ||
|| **cmd**
[`array`](../data-types.md) | An array of requests in standard format (remember to [URL-encode](./data-encoding.md) the request data; this means that the data for sub-requests must undergo double encoding) ||
|#

{% note info %}

The number of requests in a batch is limited to 50.

{% endnote %}

The array of requests can have either numeric keys or be associative. In the parameters of each subsequent request, you can use data from previous requests in the following format:

```php

$result[request_id][response_field]

```

where the request identifier is its key in the array of requests.

Starting from version **rest 24.0.0**, nesting is prohibited for the `batch` method (you cannot call another `batch` within a `batch` method call).

## Code Examples

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

- HTTP (OAuth)

    ```bash
    https://**put_your_bitrix24_address**/rest/batch?auth=d161f25928c3184678924ec127edd29a&halt=0&cmd[get_user]=user.current%3F&cmd[get_department]=department.get%3FID%3D%2524result%255Bget_user%255D%255BUF_DEPARTMENT%255D
    ```

    {% note info %}
    
    **Note**, that the parameters are URL-encoded. It is mandatory to encode parameters; otherwise, the correctness of the result is not guaranteed.
    
    {% endnote %}

- JS

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

    More about the [callBatch method in the BX24.JS SDK article](/api-reference/bx24-js-sdk/how-to-call-rest-methods/bx24-call-batch.html).

- PHP

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

## Response Handling

{% list tabs %}

- Successful Result
    ```js
    {
        "result": {
            "result": {
                "get_user": {
                    "ID": "1",
                    "ACTIVE": true,
                    "NAME": "John",
                    "LAST_NAME": "Doe",
                    "EMAIL": "my@example.com",
                    "LAST_LOGIN": "2024-08-29T10:29:54+02:00",
                    "DATE_REGISTER": "2023-08-24T03:00:00+02:00",
                    "IS_ONLINE": "Y",
                    "TIME_ZONE_OFFSET": "0",
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
                    "date_start": "2024-08-29T10:34:19+02:00",
                    "date_finish": "2024-08-29T10:34:19+02:00"
                },
                "get_department": {
                    "start": 1724916859.464944,
                    "finish": 1724916859.471518,
                    "duration": 0.006574153900146484,
                    "processing": 0.005941152572631836,
                    "date_start": "2024-08-29T10:34:19+02:00",
                    "date_finish": "2024-08-29T10:34:19+02:00"
                }
            }
        },
        "time": {
            "start": 1724916859.421475,
            "finish": 1724916859.471588,
            "duration": 0.05011296272277832,
            "processing": 0.010200977325439453,
            "date_start": "2024-08-29T10:34:19+02:00",
            "date_finish": "2024-08-29T10:34:19+02:00"
        }
    }
    ```

- Error Example (halt = 0)
    ```js
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
            "date_start": "2024-08-29T10:30:38+02:00",
            "date_finish": "2024-08-29T10:30:38+02:00"
        }
    }
    ```

- Error Example (halt = 1)
    ```js
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
            "date_start": "2024-08-29T10:32:05+02:00",
            "date_finish": "2024-08-29T10:32:05+02:00"
        }
    }
    ```

{% endlist %}

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | An object is returned with fields in the form of objects containing the results of the invoked methods ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Using Results

Requests executed by the batch method are processed sequentially and can use data obtained from previous requests.

For example, a single execution of the batch method can perform a chain of actions:

```js

BX24.callMethod(
    'batch',
    {
        'halt': 1,
        'cmd': {
            'user_by_name': 'user.search?NAME=Test2',
            'user_lead': 'crm.lead.add?fields[TITLE]=Test Assigned&fields[ASSIGNED_BY_ID]=$result[user_by_name][0][ID]'
        }
    },
    function(result)
    {
        console.log(result.answer);
    }
);

```

As a result:

- `user_by_name` — will find a user named `Test2`
- `user_lead` — will create a lead with the responsible user found in `user_by_name`

{% note info %}

**Note**, that in the `user_lead` request we use nesting `[0][ID]`. Since the `user.search` method is list-based, it can return up to 50 results, and in this case, we will take the identifier of the first returned user.

{% endnote %}

{% note warning %}

When designing a chain of commands, do not neglect the `halt` key — when enabled, it will stop the execution of the chain if one request in the chain returns an error.

{% endnote %}