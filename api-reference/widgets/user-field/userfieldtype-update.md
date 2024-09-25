# Update User Field Type Settings userfieldtype.update

> Scope: [`depending on the integration point`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `userfieldtype.update` modifies the settings of a user field type registered by the application. It returns _true_ or an error with a description of the reason.

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
|| **TITLE***
[`string`](../../data-types.md) | Text title of the type. Will be displayed in the administrative interface for user field settings | ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Text description of the type. Will be displayed in the administrative interface for user field settings | ||
|| **OPTIONS**
[`array`](../../data-types.md) | Additional settings. Currently, one key is available: `height` — specifies the height of the user field in pixels. Any positive value will apply.
Default is `0`. If `0` is specified, the standard height for displaying this integration will be used | ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "USER_TYPE_ID": "test_type",
        "HANDLER": "https://www.myapplication.com/handler/",
        "TITLE": "Updated test type",
        "DESCRIPTION": "Test user field type for documentation with updated description",
        "OPTIONS": {
            "height": 60
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldtype.update
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "USER_TYPE_ID": "test_type",
        "HANDLER": "https://www.myapplication.com/handler/",
        "TITLE": "Updated test type",
        "DESCRIPTION": "Test user field type for documentation with updated description",
        "OPTIONS": {
            "height": 60
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/userfieldtype.update
    ```

- JS

    ```js
    BX24.callMethod(
        'userfieldtype.update',
        {
            USER_TYPE_ID: 'test_type',
            HANDLER: 'https://www.myapplication.com/handler/',
            TITLE: 'Updated test type',
            DESCRIPTION: 'Test user field type for documentation with updated description',
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldtype.update',
        [
            'USER_TYPE_ID' => 'test_type',
            'HANDLER' => 'https://www.myapplication.com/handler/',
            'TITLE' => 'Updated test type',
            'DESCRIPTION' => 'Test user field type for documentation with updated description',
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
        "date_finish":"2024-08-23T16:01:51+02:00",
        "operating":0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the user field type modification ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_NOT_FOUND",
    "error_description":"User Field Type not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %} 

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ERROR_CORE` | Unable to set placement handler: Handler already binded | `HANDLER` is already occupied by another user field type of this application or `USER_TYPE_ID` is already used by another application ||
|| `ERROR_ARGUMENT` | Argument 'USER_TYPE_ID' is null or empty | `USER_TYPE_ID` is not specified ||
|| `ERROR_NOT_FOUND` | User Field Type not found | User field with the specified `USER_TYPE_ID` not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldtype-add.md)
- [{#T}](./userfieldtype-list.md)
- [{#T}](./userfieldtype-delete.md)