# Add Badge crm.activity.badge.add

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the crm section

The method `crm.activity.badge.add` adds a new badge for a configurable activity.

## Method Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **code***
[`string`](../../../../../data-types.md) | Badge code, for example `missedCall` ||
|| **title***
[`string`\|`array`](../../../../../data-types.md) | Badge title. Can be a string or an array of strings for different languages ||
|| **value***
[`string`\|`array`](../../../../../data-types.md) | Badge value. Can be a string or an array of strings for different languages ||
|| **type***
[`string`](../../../../../data-types.md) | [Badge type](./index.md#badge-type) ||
|#

## Code Examples

{% include [Note on examples](../../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"code":"missedCall","title":"Call Status","value":"Missed","type":"failure","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.badge.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.activity.badge.add',
            {
                code: 'missedCall',
                title: 'Call Status',
                value: 'Missed',
                type: 'failure'
            }
        );
        
        const result = response.getData().result;
        if (result.error())
            console.error(result.error());
        else
            console.dir(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.badge.add',
                [
                    'code'  => 'missedCall',
                    'title' => 'Call Status',
                    'value' => 'Missed',
                    'type'  => 'failure'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding activity badge: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.activity.badge.add",
        {
            code: 'missedCall',
            title: 'Call Status',
            value: 'Missed',
            type: 'failure'
        }, result => {
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
        'crm.activity.badge.add',
        [
            'code' => 'missedCall',
            'title' => 'Call Status',
            'value' => 'Missed',
            'type' => 'failure'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "badge": {
            "code": "missedCall",
            "title": "Call Status",
            "value": "Missed",
            "type": "failure"
        }
    },
    "time": {
        "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2025-01-21T13:47:08+02:00",
        "date_finish": "2025-01-21T13:47:08+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Root element of the response containing information about the added badge in case of success. In case of failure, it will return `null` ||
|| **time**
[`time`](../../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `REQUIRED_ARG_MISSING` | Required fields are not filled ||
|#

{% include [system errors](../../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-badge-get.md)
- [{#T}](./crm-activity-badge-list.md)
- [{#T}](./crm-activity-badge-delete.md)