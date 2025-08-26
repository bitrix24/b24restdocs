# Change parameters of the mail service mailservice.update

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing descriptions of parameter types
- no response examples
- parameter versions not specified

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.update` updates the parameters of the mail service.

## Parameters

#|
||  **Parameter** / **Type**| **Description** | **Available from** ||
|| **ID**
[`unknown`](../data-types.md) | Identifier of the mail service | ||
|| **ACTIVE**
[`unknown`](../data-types.md) | Service activity (Y / N) | ||
|| **NAME**
[`unknown`](../data-types.md) | Name of the added mail service | ||
|| **SERVER**
[`unknown`](../data-types.md) | Address of the added server | ||
|| **PORT**
[`unknown`](../data-types.md) | Port number | ||
|| **ENCRYPTION**
[`unknown`](../data-types.md) | Need for encryption (Y / N) | ||
|| **LINK**
[`unknown`](../data-types.md) | Link to the mail service | ||
|| **SORT**
[`unknown`](../data-types.md) | Sort index | ||
|#

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"mailservice.update",
    		{
    			'ID': 5,
    			'ACTIVE': 'N',
    			'NAME': 'Mail service Yandex',
    			'SERVER': 'imap.yandex.com',
    			'PORT': '993',
    			'ENCRYPTION': 'Y',
    			'LINK': 'https://mail.yandex.com/',
    			'SORT': '666'
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'mailservice.update',
                [
                    'ID'        => 5,
                    'ACTIVE'    => 'N',
                    'NAME'      => 'Mail service Yandex',
                    'SERVER'    => 'imap.yandex.com',
                    'PORT'      => '993',
                    'ENCRYPTION' => 'Y',
                    'LINK'      => 'https://mail.yandex.com/',
                    'SORT'      => '666',
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
        echo 'Error updating mail service: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "mailservice.update",
        {
            'ID': 5,
            'ACTIVE': 'N',
            'NAME': 'Mail service Yandex',
            'SERVER': 'imap.yandex.com',
            'PORT': '993',
            'ENCRYPTION': 'Y',
            'LINK': 'https://mail.yandex.com/',
            'SORT': '666'
        },
        function(result)
        {
            if(result.error())
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

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}