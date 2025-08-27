# Get main calendar settings calendar.settings.get

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves the main calendar settings. Only an administrator of the account can modify the main settings.

No parameters.

## Code Examples

{% include [Footnote about examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.settings.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.settings.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'calendar.settings.get',
    		{}
    	);
    	
    	const result = response.getData().result;
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
                'calendar.settings.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting calendar settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.settings.get',
        {}
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.settings.get',
        []
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
        "work_time_start": "9",
        "work_time_end": "19",
        "year_holidays": "1.01,2.01,7.01,23.02,8.03,1.05,9.05,12.06,4.11",
        "year_workdays": "31.12",
        "week_holidays": [
            "SA",
            "SU"
        ],
        "week_start": "MO",
        "user_name_template": "#NAME# #LAST_NAME#",
        "sync_by_push": false,
        "user_show_login": true,
        "path_to_user": "/company/personal/user/#user_id#/",
        "path_to_user_calendar": "/company/personal/user/#user_id#/calendar/",
        "path_to_group": "/workgroups/group/#group_id#/",
        "path_to_group_calendar": "/workgroups/group/#group_id#/calendar/",
        "path_to_vr": "",
        "path_to_rm": "",
        "rm_iblock_type": "",
        "rm_iblock_id": "",
        "dep_manager_sub": true,
        "denied_superpose_types": [],
        "pathes_for_sites": "",
        "forum_id": "8",
        "rm_for_sites": true,
        "path_to_type_company_calendar": "",
        "path_to_type_location": "",
        "path_to_type_open_event": ""
    },
    "time": {
        "start": 1733924639.802569,
        "finish": 1733924640.184363,
        "duration": 0.3817939758300781,
        "processing": 0.012382984161376953,
        "date_start": "2024-12-11T13:43:59+00:00",
        "date_finish": "2024-12-11T13:44:00+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response ||
|| **work_time_start**
[`string`](../data-types.md) | Start time of the workday ||
|| **work_time_end**
[`string`](../data-types.md) | End time of the workday ||
|| **year_holidays**
[`string`](../data-types.md) | List of holidays ||
|| **week_holidays**
[`array`](../data-types.md) | Array of weekend days ||
|| **week_start**
[`string`](../data-types.md) | Day the week starts ||
|| **user_name_template**
[`string`](../data-types.md) | User name template ||
|| **sync_by_push**
[`boolean`](../data-types.md) | Flag for automatic calendar synchronization via subscription. Push events from Google/Office365 ||
|| **user_show_login**
[`boolean`](../data-types.md) | Flag for displaying user login ||
|| **path_to_user**
[`string`](../data-types.md) | Template link to user profile ||
|| **path_to_user_calendar**
[`string`](../data-types.md) | Template link to view user calendar ||
|| **path_to_group**
[`string`](../data-types.md) | Template link to view workgroup ||
|| **path_to_group_calendar**
[`string`](../data-types.md) | Template link to view group calendar ||
|| **path_to_vr**
[`string`](../data-types.md) | Template link to video conference room ||
|| **path_to_rm**
[`string`](../data-types.md) | Template link to meeting room ||
|| **rm_iblock_type**
[`string`](../data-types.md) | Type of infoblock for booking meeting and video conference rooms ||
|| **rm_iblock_id**
[`string`](../data-types.md) | Identifier of the infoblock for booking meeting rooms ||
|| **dep_manager_sub**
[`boolean`](../data-types.md) | Flag allowing managers to view subordinates' calendars ||
|| **denied_superpose_types**
[`array`](../data-types.md) | List of calendar types that cannot be added to favorites ||
|| **pathes_for_sites**
[`boolean`](../data-types.md) | Sets link templates common for all sites ||
|| **forum_id**
[`string`](../data-types.md) | Identifier of the forum for comments ||
|| **rm_for_sites**
[`boolean`](../data-types.md) | Sets meeting room parameters common for all sites ||
|| **path_to_type_company_calendar**
[`string`](../data-types.md) | Template link to view company calendars ||
|| **path_to_type_location**
[`string`](../data-types.md) | Template link to view meeting room bookings ||
|| **path_to_type_open_event**
[`string`](../data-types.md) | Template link to view open event calendar ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-user-settings-get.md)
- [{#T}](./calendar-user-settings-set.md)