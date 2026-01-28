# Reset Company Card Settings crm.company.details.configuration.reset

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
>  - any user can reset their settings,
>  - a user with the "Allow to change settings" access permission in CRM can reset both general and other users' settings.

{% note warning "Method development halted" %}

The method `crm.company.details.configuration.reset` continues to function, but there is a more relevant alternative [crm.item.details.configuration.reset](../../universal/item-details-configuration/crm-item-details-configuration-reset.md).

{% endnote %}

The method `crm.company.details.configuration.reset` resets the settings of company cards: it removes personal settings for the specified user or general settings defined for all users.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings.

Possible values:
- `P` — personal settings
- `C` — general settings

Default is `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier, which can be obtained using the [user.get](../../../user/user-get.md) method. Required only when resetting personal settings.

If not specified, the `id` of the current user is used.
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

1. Reset personal card settings

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":1}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.details.configuration.reset
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":1,"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.company.details.configuration.reset
        ```

    - JS

        ```js
        try
        {
        	const response = await $b24.callMethod(
        		"crm.company.details.configuration.reset",
        		{
        			scope: "P",
        			userId: 1
        		}
        	);
        	
        	const result = response.getData().result;
        	console.dir(result);
        }
        catch(error)
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
                    'crm.company.details.configuration.reset',
                    [
                        'scope'  => 'P',
                        'userId' => 1,
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
            echo 'Error resetting company details configuration: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            "crm.company.details.configuration.reset",
            {
                scope: "P",
                userId: 1
            },
            function(result)
            {
                if(result.error())
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
            'crm.company.details.configuration.reset',
            [
                'scope' => 'P',
                'userId' => 1
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Reset general card settings

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C"}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.details.configuration.reset
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C","auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.company.details.configuration.reset
        ```

    - JS

        ```js
        try
        {
        	const response = await $b24.callMethod(
        		"crm.company.details.configuration.reset",
        		{
        			scope: "C"
        		}
        	);
        	
        	const result = response.getData().result;
        	console.dir(result);
        }
        catch(error)
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
                    'crm.company.details.configuration.reset',
                    [
                        'scope'  => 'C',
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
            echo 'Error resetting company details configuration: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            "crm.company.details.configuration.reset",
            {
                scope: "C"
            },
            function(result)
            {
                if(result.error())
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
            'crm.company.details.configuration.reset',
            [
                'scope' => 'C'
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
        "start": 1769420530,
        "finish": 1769420530.756109,
        "duration": 0.7561089992523193,
        "processing": 0,
        "date_start": "2026-01-26T12:42:10+01:00",
        "date_finish": "2026-01-26T12:42:10+01:00",
        "operating_reset_at": 1769421130,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The root element of the response.

Returns `true` if the settings were successfully reset ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `Access denied` | The user does not have permission to reset settings ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-company-details-configuration-get.md)
- [{#T}](.//crm-company-details-configuration-set.md)
- [{#T}](./crm-company-details-configuration-force-common-scope-for-all.md)