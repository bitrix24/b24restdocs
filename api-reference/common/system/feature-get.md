# Get Information on Feature Availability on Account feature.get

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `feature.get` returns information about the availability of features on a specific account.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../data-types.md) | Available keys:
- `rest_offline_extended` — availability of offline events
- `rest_auth_connector` — availability of the `auth_connector` key in events ||
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
        "CODE": "rest_offline_extended"
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/feature.get
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "CODE": "rest_offline_extended",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/feature.get
    ```

- JS

    ```js
    BX24.callMethod(
        "feature.get",
        {
            "CODE": "rest_offline_extended"
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
        'feature.get',
        [
            'CODE' => 'rest_offline_extended'
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
    "result": {
        "value": "Y"
    },
    "time": {
        "start": 1722434594.84942,
        "finish": 1722434594.90542,
        "duration": 0.0560011863708496,
        "processing": 0.000065088272094726,
        "date_start": "2024-07-31T14:03:14+00:00",
        "date_finish": "2024-07-31T14:03:14+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The object contains information about the availability of the method:
- `value` — (Y/N) presence of the feature on the account
- `lang_selfhosted` — *lang* is replaced with en, de, ua, kz, etc. (used for on-premise *Bitrix24*) ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"CODE_EMPTY",
    "error_description":"CODE can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %} 

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `CODE_EMPTY` | CODE can't be empty | The CODE parameter was not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./method-get.md)
- [{#T}](./scope.md)
- [{#T}](./app-info.md)
- [{#T}](./access-name.md)
- [{#T}](./server-time.md)
- [{#T}](./methods.md)