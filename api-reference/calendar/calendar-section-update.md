# Update Calendar calendar.section.update

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the calendar.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../data-types.md) | Calendar type. Possible values:
- `user` — user calendar
- `group` — group calendar  ||
|| **ownerId***
[`integer`](../data-types.md) | Identifier of the calendar owner ||
|| **id***
[`string`](../data-types.md) | Identifier of the calendar ||
|| **name**
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
[`boolean`](../data-types.md) | Allow calendar export ||
|| **SET**
[`string`](../data-types.md) | Period for which to perform the export. Possible values:
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
    -d '{"id":325,"type":"user","ownerId":2,"name":"Changed Section Name","description":"New description for section","color":"#9cbeAA","text_color":"#283099","export":[{"ALLOW":false}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.section.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":325,"type":"user","ownerId":2,"name":"Changed Section Name","description":"New description for section","color":"#9cbeAA","text_color":"#283099","export":[{"ALLOW":false}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.section.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'calendar.section.update',
    		{
    			id: 325,
    			type: 'user',
    			ownerId: 2,
    			name: 'Changed Section Name',
    			description: 'New description for section',
    			color: '#9cbeAA',
    			text_color: '#283099',
    			export: [
    				{
    					ALLOW: false
    				}
    			]
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Updated calendar section with ID:', result);
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
                'calendar.section.update',
                [
                    'id'          => 325,
                    'type'        => 'user',
                    'ownerId'     => 2,
                    'name'        => 'Changed Section Name',
                    'description' => 'New description for section',
                    'color'       => '#9cbeAA',
                    'text_color'  => '#283099',
                    'export'      => [
                        [
                            'ALLOW' => false
                        ]
                    ]
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
        echo 'Error updating calendar section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.section.update',
        {
            id: 325,
            type: 'user',
            ownerId: 2,
            name: 'Changed Section Name',
            description: 'New description for section',
            color: '#9cbeAA',
            text_color: '#283099',
            export: [
                {
                    ALLOW: false
                }
            ]
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.section.update',
        [
            'id' => 325,
            'type' => 'user',
            'ownerId' => 2,
            'name' => 'Changed Section Name',
            'description' => 'New description for section',
            'color' => '#9cbeAA',
            'text_color' => '#283099',
            'export' => [
                [
                    'ALLOW' => false
                ]
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
[`integer`](../data-types.md) | Identifier of the updated calendar ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "type" for the method "calendar.section.update" is not set"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "type" for the method "calendar.section.update" is not set | The required parameter `type` is missing ||
|| Empty string | The required parameter "ownerId" for the method "calendar.section.update" is not set | The required parameter `ownerId` is missing and the `type` parameter is not equal to 'user' ||
|| Empty string | Section ID is not set | The required parameter `id` is missing ||
|| Empty string | Invalid value for the "name" parameter | Incorrect data format in the `name` field ||
|| Empty string | Invalid value for the "description" parameter | Incorrect data format in the `description` field ||
|| Empty string | Access denied | The calendar with the specified `id` does not exist or there are no rights to edit the calendar ||
|| Empty string | An error occurred while updating the section | Another error ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-section-add.md)
- [{#T}](./calendar-section-get.md)
- [{#T}](./calendar-section-delete.md)