# Register a new user field type userfieldtype.add

> Scope: [`depending on the embedding location`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `userfieldtype.add` method registers a new type of custom fields. After registering the type, create a custom field using the [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-add.md) method.

When opening a card with a custom type field, an array `PLACEMENT_OPTIONS` containing data about the field and entity is passed to the application handler.

```json
{
    "MODE": "view",
    "ENTITY_ID": "CRM_DEAL",
    "FIELD_NAME": "UF_CRM_TEST_TYPE_1",
    "ENTITY_VALUE_ID": "7303",
    "VALUE": null,
    "MULTIPLE": "N",
    "MANDATORY": "N",
    "XML_ID": null,
    "ENTITY_DATA": {
        "entityTypeId": 2,
        "entityId": "7303",
        "module": "crm"
    }
}
```

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** | **Restrictions** ||
|| **USER_TYPE_ID***
[`string`](../../data-types.md) | String code for the type | 
- a-z0-9
- must be unique ||
|| **HANDLER***
[`URL`](../../data-types.md) | Address of the user type handler | 
- in the same domain as the main application address
- must be unique ||
|| **TITLE**
[`string`](../../data-types.md) | Text name of the type. Will be displayed in the administrative interface for user field settings | ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Text description of the type. Will be displayed in the administrative interface for user field settings | ||
|| **OPTIONS**
[`array`](../../data-types.md) | Additional settings. Currently, one key is available: `height` — specifies the height of the user field in pixels. Any positive value will apply.
Default is `0`. If `0` is specified, the standard height for displaying this embedding will be used | ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "USER_TYPE_ID": "test_type",
        "HANDLER": "https://www.myapplication.com/handler/",
        "TITLE": "Updated test type",
        "DESCRIPTION": "Test userfield type for documentation with updated description",
        "OPTIONS": {
            "height": 60
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldtype.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "USER_TYPE_ID": "test_type",
        "HANDLER": "https://www.myapplication.com/handler/",
        "TITLE": "Updated test type",
        "DESCRIPTION": "Test userfield type for documentation with updated description",
        "OPTIONS": {
            "height": 60
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/userfieldtype.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'userfieldtype.add',
    		{
    			USER_TYPE_ID: 'test_type',
    			HANDLER: 'https://www.myapplication.com/handler/',
    			TITLE: 'Updated test type',
    			DESCRIPTION: 'Test userfield type for documentation with updated description',
    			OPTIONS: {
    				height: 60,
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
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
                'userfieldtype.add',
                [
                    'USER_TYPE_ID' => 'test_type',
                    'HANDLER'      => 'https://www.myapplication.com/handler/',
                    'TITLE'        => 'Updated test type',
                    'DESCRIPTION'  => 'Test userfield type for documentation with updated description',
                    'OPTIONS'      => [
                        'height' => 60,
                    ],
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
        echo 'Error adding user field type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'userfieldtype.add',
        {
            USER_TYPE_ID: 'test_type',
            HANDLER: 'https://www.myapplication.com/handler/',
            TITLE: 'Updated test type',
            DESCRIPTION: 'Test userfield type for documentation with updated description',
            OPTIONS: {
                height: 60,
            },
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldtype.add',
        [
            'USER_TYPE_ID' => 'test_type',
            'HANDLER' => 'https://www.myapplication.com/handler/',
            'TITLE' => 'Updated test type',
            'DESCRIPTION' => 'Test userfield type for documentation with updated description',
            'OPTIONS' => [
                'height' => 60
            ]
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
    "result":true,
    "time":{
        "start":1724421710.397825,
        "finish":1724421711.040353,
        "duration":0.6425280570983887,
        "processing":5.888938903808594e-5,
        "date_start":"2024-08-23T16:01:50+02:00",
        "date_finish":"2024-08-23T16:01:51+02:00","operating":0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of registering the new user field type ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"Unable to set placement handler: Handler already binded"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %} 

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ERROR_CORE` | Unable to set placement handler: Handler already binded | `HANDLER` is already occupied by another user field type of this application or `USER_TYPE_ID` is already used by another application ||
|| `ERROR_ARGUMENT` | Argument 'USER_TYPE_ID' is null or empty | `USER_TYPE_ID` is not specified ||
|| `ERROR_ARGUMENT` | Argument 'HANDLER' is null or empty | `HANDLER` is not specified ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldtype-update.md)
- [{#T}](./userfieldtype-list.md)
- [{#T}](./userfieldtype-delete.md)