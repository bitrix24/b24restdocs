# Remove Registered Automation Rule Handler placement.unbind

> Scope: [`placement`](../scopes/permissions.md)
>
> Who can execute the method: administrator, authorized in the application

The method `placement.unbind` removes the registered Automation rule handler.

{% note info "" %}

The method works only in the context of the [application](../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT**^*^
[`string`](../data-types.md) | Identifier of the Automation rule placement.

`PLACEMENT` can be obtained by:
- using the [placement.list](./placement-list.md) method
- using the [placement.get](./placement-get.md) method in the `placement` field ||
|| **HANDLER**
[`string`](../data-types.md) | URL of the Automation rule handler.

`HANDLER` can be obtained using the [placement.get](./placement-get.md) method in the `handler` field.

If the parameter is not provided or is empty, the method removes all handlers for the specified Automation rule registered by the application. ||
|| **USER_ID**
[`integer`](../data-types.md) | Identifier of the Bitrix24 user for whom the handler was registered.

`USER_ID` can be obtained by:
- using the [user.get](../user/user-get.md) method
- using the [user.current](../user/user-current.md) method for the current user

If the parameter is provided, the method removes only the handlers for the specified user. ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

Example of removing a registered Automation rule handler, where:
- `PLACEMENT` — identifier of the Automation rule placement
- `HANDLER` — URL of the Automation rule handler

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "PLACEMENT": "CRM_LEAD_DETAIL_TAB",
        "HANDLER": "https://www.myapplicationhost.com/placement/",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/placement.unbind.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'placement.unbind',
    		{
    			PLACEMENT: 'CRM_LEAD_DETAIL_TAB',
    			HANDLER: 'https://www.myapplicationhost.com/placement/'
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
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
                'placement.unbind',
                [
                    'PLACEMENT' => 'CRM_LEAD_DETAIL_TAB',
                    'HANDLER' => 'https://www.myapplicationhost.com/placement/',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error unbinding placement: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'placement.unbind',
        {
            PLACEMENT: 'CRM_LEAD_DETAIL_TAB',
            HANDLER: 'https://www.myapplicationhost.com/placement/'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'placement.unbind',
        [
            'PLACEMENT' => 'CRM_LEAD_DETAIL_TAB',
            'HANDLER' => 'https://www.myapplicationhost.com/placement/',
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "count": 4
    },
    "time": {
        "start": 1775058296,
        "finish": 1775058296.998083,
        "duration": 0.9980831146240234,
        "processing": 0,
        "date_start": "2026-04-01T18:44:56+02:00",
        "date_finish": "2026-04-01T18:44:56+02:00",
        "operating_reset_at": 1775058896,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with the result of the removal:

- **count** [`integer`](../data-types.md) — number that the method increments:
  - by `1` for each found handler record before the removal call
  - by another `1` after each successful removal ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | Method call not from application context ||
|| `403` | `ACCESS_DENIED` | Access denied! | User is not an administrator ||
|| `400` | `ERROR_ARGUMENT` | Argument 'PLACEMENT' is null or empty | 'PLACEMENT' not provided or empty value passed ||
|| `400` | `ERROR_ARGUMENT` | The value of an argument 'PLACEMENT' must be of type string | 'PLACEMENT' parameter passed is not a string ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./placements.md)
- [{#T}](./placement-list.md)
- [{#T}](./placement-bind.md)
- [{#T}](./placement-get.md)
- [{#T}](./ui-interaction/index.md)