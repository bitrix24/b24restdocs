# Set Time Control Settings timeman.timecontrol.settings.set

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `timeman.timecontrol.settings.set` sets the settings for the time control module.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **ACTIVE**
[`boolean`](../../data-types.md) | Activate the time control module. Possible values:
- `true` or `1` — enable the module
- `0` — disable the module. The option `'ACTIVE': false` does not disable the module
||
|| **MINIMUM_IDLE_FOR_REPORT**
[`integer`](../../data-types.md) | Minimum idle time in minutes after which a report is required ||
|| **REGISTER_OFFLINE**
[`boolean`](../../data-types.md) | Register offline status ||
|| **REGISTER_IDLE**
[`boolean`](../../data-types.md) | Register idle status ||
|| **REGISTER_DESKTOP**
[`boolean`](../../data-types.md) | Register desktop application status ||
|| **REPORT_REQUEST_TYPE**
[`string`](../../data-types.md) | Type of report request. Possible values:
- `all` — for everyone
- `user` — for specific users
- `none` — for no one ||
|| **REPORT_REQUEST_USERS**
[`array`](../../data-types.md) | Array of user IDs for whom report requests are required.

Filled if `REPORT_REQUEST_USERS` is set to `user` ||
|| **REPORT_SIMPLE_TYPE**
[`string`](../../data-types.md) | Type of simple report. Possible values:
- `all` — for everyone
- `user` — for specific users
- `none` — for no one ||
|| **REPORT_SIMPLE_USERS**
[`array`](../../data-types.md) | Array of user IDs with access to the simple report.

Filled if `REPORT_SIMPLE_USERS` is set to `user` ||
|| **REPORT_FULL_TYPE**
[`string`](../../data-types.md) | Type of full report. Possible values:
- `all` — for everyone
- `user` — for specific users
- `none` — for no one ||
|| **REPORT_FULL_USERS**
[`array`](../../data-types.md) | Array of user IDs with access to the full report.

Filled if `REPORT_FULL_USERS` is set to `user`  ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ACTIVE":true,"MINIMUM_IDLE_FOR_REPORT":15,"REGISTER_OFFLINE":true,"REGISTER_IDLE":true,"REGISTER_DESKTOP":true,"REPORT_REQUEST_TYPE":"all","REPORT_SIMPLE_TYPE":"all","REPORT_FULL_TYPE":"all"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.timecontrol.settings.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ACTIVE":true,"MINIMUM_IDLE_FOR_REPORT":15,"REGISTER_OFFLINE":true,"REGISTER_IDLE":true,"REGISTER_DESKTOP":true,"REPORT_REQUEST_TYPE":"all","REPORT_SIMPLE_TYPE":"all","REPORT_FULL_TYPE":"all","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.timecontrol.settings.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'timeman.timecontrol.settings.set',
    		{
    			'ACTIVE': true,
    			'MINIMUM_IDLE_FOR_REPORT': 15,
    			'REGISTER_OFFLINE': true,
    			'REGISTER_IDLE': true,
    			'REGISTER_DESKTOP': true,
    			'REPORT_REQUEST_TYPE': 'all',
    			'REPORT_SIMPLE_TYPE': 'all',
    			'REPORT_FULL_TYPE': 'all'
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.timecontrol.settings.set',
                [
                    'ACTIVE'                 => true,
                    'MINIMUM_IDLE_FOR_REPORT' => 15,
                    'REGISTER_OFFLINE'        => true,
                    'REGISTER_IDLE'           => true,
                    'REGISTER_DESKTOP'        => true,
                    'REPORT_REQUEST_TYPE'     => 'all',
                    'REPORT_SIMPLE_TYPE'      => 'all',
                    'REPORT_FULL_TYPE'        => 'all',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting time control settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'timeman.timecontrol.settings.set',
        {
            'ACTIVE': true,
            'MINIMUM_IDLE_FOR_REPORT': 15,
            'REGISTER_OFFLINE': true,
            'REGISTER_IDLE': true,
            'REGISTER_DESKTOP': true,
            'REPORT_REQUEST_TYPE': 'all',
            'REPORT_SIMPLE_TYPE': 'all',
            'REPORT_FULL_TYPE': 'all'
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
        'timeman.timecontrol.settings.set',
        [
            'ACTIVE' => true,
            'MINIMUM_IDLE_FOR_REPORT' => 15,
            'REGISTER_OFFLINE' => true,
            'REGISTER_IDLE' => true,
            'REGISTER_DESKTOP' => true,
            'REPORT_REQUEST_TYPE' => 'all',
            'REPORT_SIMPLE_TYPE' => 'all',
            'REPORT_FULL_TYPE' => 'all'
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
    "result": true,
    "time": {
        "start": 1748526089.625516,
        "finish": 1748526089.656787,
        "duration": 0.03127098083496094,
        "processing": 0.008746147155761719,
        "date_start": "2025-05-29T16:41:29+02:00",
        "date_finish": "2025-05-29T16:41:29+02:00",
        "operating_reset_at": 1748526689,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Execution result.

Returns `true` if the settings are successfully saved ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to use this method"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_ERROR` | You don't have access to use this method | You do not have access to this method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-timecontrol-report-add.md)
- [{#T}](./timeman-timecontrol-reports-get.md)
- [{#T}](./timeman-timecontrol-reports-users-get.md)