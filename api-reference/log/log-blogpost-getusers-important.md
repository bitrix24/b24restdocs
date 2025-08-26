# View users who read an important message log.blogpost.getusers.important

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of error
- no examples in other languages
- the description needs a link to the user documentation on important messages

{% endnote %}

{% endif %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: any user

Returns an array of user IDs who have read the specified important message.

#|
|| **Parameter** | **Description** ||
|| **POST_ID** | ID of the news feed message that is an important message. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'log.blogpost.getusers.important',
    		{
    			POST_ID: 345
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
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
                'log.blogpost.getusers.important',
                [
                    'POST_ID' => 345
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
        echo 'Error getting important blog post users: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'log.blogpost.getusers.important',
        {
            POST_ID: 345
        },
        function(result)
        {
            console.log(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}

## Request:

{% list tabs %}

- URL request

    ```http
    https://my.bitrix24.com/rest/log.blogpost.getusers.important.json?POST_ID=345&auth=xxxxxxx
    ```

{% endlist %}

## Response:

> 200 OK

```json
{"result":["1","2","3"]}
```