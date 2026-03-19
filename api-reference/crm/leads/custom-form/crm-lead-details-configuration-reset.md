# Reset CRM Lead Details Configuration Parameters

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
>  - any user can reset their own settings,
>  - a user with the "Allow to modify settings" access permission in CRM can reset both general and others' settings.

{% note warning "Method Development Stopped" %}

The method `crm.lead.details.configuration.reset` continues to function, but there is a more relevant alternative: [crm.item.details.configuration.reset](../../universal/item-details-configuration/crm-item-details-configuration-reset.md).

{% endnote %}

The method `crm.lead.details.configuration.reset` resets the settings of lead cards.

{% note warning %}

The settings for repeat leads may differ from those for simple leads. To switch between lead card settings, use the `lead.customer.type` parameter in `extras`.

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **userId**
[`user`](../../../data-types.md) | The identifier of the user for whom the personal configuration needs to be reset.

If the parameter is not provided, the `userId` of the user calling the method will be used.

This is only required when resetting personal settings. ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings. Possible values:
- `'P'` - personal settings
- `'C'` - general settings

The default value is `'P'`. ||
|| **extras**
[`object`](../../../data-types.md) | Additional parameters for selecting the type of lead. The structure is described [below](#extras) ||
|#

### Extras Parameter {#extras}

#|
|| **Name**
`type` | **Description** ||
|| **lead.customer.type**
[`integer`](../../../data-types.md) | Type of lead. Possible values:
- `1` - simple lead
- `2` - repeat lead ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

1. Reset Personal Card Settings

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":1}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.details.configuration.reset
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":1,"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.lead.details.configuration.reset
        ```

    - JS

        ```js
        try
        {
            const response = await $b24.callMethod(
                'crm.lead.details.configuration.reset',
                {
                    scope: 'P',
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
                    'crm.lead.details.configuration.reset',
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
            echo 'Error resetting lead details configuration: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            "crm.lead.details.configuration.reset",
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
            'crm.lead.details.configuration.reset',
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

2. Reset General Card Settings

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C"}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.details.configuration.reset
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C","auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.lead.details.configuration.reset
        ```

    - JS

        ```js
        try
        {
        	const response = await $b24.callMethod(
        		"crm.lead.details.configuration.reset",
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
                    'crm.lead.details.configuration.reset',
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
            echo 'Error resetting lead details configuration: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            "crm.lead.details.configuration.reset",
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
            'crm.lead.details.configuration.reset',
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
        "start": 1720687072.190654,
        "finish": 1720687072.586945,
        "duration": 0.39629101753234863,
        "processing": 0.057084083557128906,
        "date_start": "2024-07-11T10:37:52+02:00",
        "date_finish": "2024-07-11T10:37:52+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The root element of the response. Returns `true` if the settings were successfully reset. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | Access denied | Insufficient permissions to reset the requested configuration. ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-lead-details-configuration-get.md)
- [{#T}](./crm-lead-details-configuration-set.md)
- [{#T}](./crm-lead-details-configuration-force-common-scope-for-all.md)