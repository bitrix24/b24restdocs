# Add Calendar calendar.section.add

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a new calendar.

The system will add a new calendar only for the user who executes the method. An administrator can create calendars for other users.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../data-types.md) | Calendar type. Possible values: 
- `user` — user calendar 
- `group` — group calendar ||
|| **ownerId***
[`integer`](../data-types.md) | Identifier of the calendar owner.

For `type` with the value `user`, the identifier of the current user will be set if no value is passed in `ownerId` ||
|| **name***
[`string`](../data-types.md) | Calendar name ||
|| **description**
[`string`](../data-types.md) | Calendar description ||
|| **color**
[`string`](../data-types.md) | Calendar color ||
|| **text_color**
[`string`](../data-types.md) | Text color in the calendar ||
|| **export**
[`object`](../data-types.md) | Object [export parameters for the calendar](#export)
||
|#

### Export Parameter {#export}

#|
|| **Name**
`type` | **Description** ||
|| **ALLOW**
[`boolean`](../data-types.md) | Allow calendar export. Possible values:
- `true` — allow
- `false` — disallow ||
|| **SET**
[`string`](../data-types.md) | Period for export. Possible values:
- `all` — for the entire period
- `3_9` — 3 months before and 9 after
- `6_12` — 6 months before and 12 after
 ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":2,"name":"New Section","description":"Description for section","color":"#9cbeee","text_color":"#283000","export":{"ALLOW":false,"SET":"3_9"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.section.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":2,"name":"New Section","description":"Description for section","color":"#9cbeee","text_color":"#283000","export":{"ALLOW":false,"SET":"3_9"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.section.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'calendar.section.add',
    		{
    			type: 'user',
    			ownerId: 2,
    			name: 'New Section',
    			description: 'Description for section',
    			color: '#9cbeee',
    			text_color: '#283000',
    			export: {
    				ALLOW: false,
    				SET: '3_9'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Created element with ID:', result);
    	// Your logic for processing data
    	processResult(result);
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
                'calendar.section.add',
                [
                    'type'        => 'user',
                    'ownerId'     => 2,
                    'name'        => 'New Section',
                    'description' => 'Description for section',
                    'color'       => '#9cbeee',
                    'text_color'  => '#283000',
                    'export'      => [
                        'ALLOW' => false,
                        'SET'   => '3_9',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding calendar section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.section.add',
        {
            type: 'user',
            ownerId: 2,
            name: 'New Section',
            description: 'Description for section',
            color: '#9cbeee',
            text_color: '#283000',
            export: {
                ALLOW: false,
                SET: '3_9'
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.section.add',
        [
            'type' => 'user',
            'ownerId' => 2,
            'name' => 'New Section',
            'description' => 'Description for section',
            'color' => '#9cbeee',
            'text_color' => '#283000',
            'export' => [
                'ALLOW' => false,
                'SET' => '3_9'
            ]
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
    "result": 190,
    "time": {
        "start": 1733812564.64201,
        "finish": 1733812565.71673,
        "duration": 1.0747201442718506,
        "processing": 0.05963897705078125,
        "date_start": "2024-12-08T06:36:04+00:00",
        "date_finish": "2024-12-08T06:36:05+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the created calendar ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "type" for the method "calendar.section.add" is not set."
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "type" for the method "calendar.section.add" is not set. | The required parameter `type` is not provided ||
|| Empty string | The required parameter "ownerId" for the method "calendar.section.add" is not set. | The required parameter `ownerId` is not provided and the `type` parameter is not equal to `user` ||
|| Empty string | Invalid value for the "name" parameter | Incorrect data format in the `name` field ||
|| Empty string | Invalid value for the "description" parameter | Incorrect data format in the `description` field ||
|| Empty string | Access denied | No permission to create a calendar with the provided `type` ||
|| Empty string | An error occurred while creating the section | Another error ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-section-update.md)
- [{#T}](./calendar-section-get.md)
- [{#T}](./calendar-section-delete.md)