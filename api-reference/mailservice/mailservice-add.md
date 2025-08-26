# Create a new mail service mailservice.add

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameter type descriptions
- no response examples
- parameter versions not specified

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.add` adds a new mail service.

## Parameters

#|
||  **Parameter** / **Type**| **Description** | **Available since** ||
|| **ACTIVE**
[`unknown`](../data-types.md) | Service activity (Y / N) | ||
|| **NAME**
[`unknown`](../data-types.md) | Name of the mail service being added | ||
|| **SERVER**
[`unknown`](../data-types.md) | Address of the mail server being added | ||
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
    		"mailservice.add",
    		{
    			'ACTIVE': 'Y',
    			'NAME': 'My mail service',
    			'SERVER': 'imap.my-mail.com',
    			'PORT': '993',
    			'ENCRYPTION': 'Y',
    			'LINK': 'https://mail.my-mail.com/',
    			'SORT': '500'
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'mailservice.add',
                [
                    'ACTIVE'     => 'Y',
                    'NAME'       => 'My mail service',
                    'SERVER'     => 'imap.my-mail.com',
                    'PORT'       => '993',
                    'ENCRYPTION' => 'Y',
                    'LINK'       => 'https://mail.my-mail.com/',
                    'SORT'       => '500',
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
        echo 'Error adding mail service: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "mailservice.add",
        {
            'ACTIVE': 'Y',
            'NAME': 'My mail service',
            'SERVER': 'imap.my-mail.com',
            'PORT': '993',
            'ENCRYPTION': 'Y',
            'LINK': 'https://mail.my-mail.com/',
            'SORT': '500'
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

{% include [Examples note](../../_includes/examples.md) %}