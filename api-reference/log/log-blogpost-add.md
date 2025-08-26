# Add a message to the News Feed on behalf of the current user log.blogpost.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no error response is provided
- No examples in other languages

{% endnote %}

{% endif %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

Adds a message to the **News Feed** on behalf of the current user.

## Request:

```http
https://my.bitrix24.com/rest/log.blogpost.add.json?POST_MESSAGE=Hello%2C%20world!&auth=d9a76e2929b7bc1ff21aee9c0ce7e3e2
```

## Response:

```json
{"result":true}
```

## Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_ID** | ID of the message author (optional, defaults to the current user, other values are available only to the administrator in the on-premise version). ||
|| **POST_MESSAGE** | Message text. ||
|| **POST_TITLE** | Message title. ||
|| **DEST** | List of recipients who will have the right to view the message. Possible values for array elements:

{% include notitle [message recipients](./_includes/log-recepients.md) %}

Default value - `['UA']` ||
|| **SPERM** | List of recipients who will have the right to view the message (deprecated). Similar to `DEST` ||
|| **FILES** | Files, an array of values described by [rules](../files/how-to-upload-files.md).||
|| **IMPORTANT** | Defaults to N. The feed message is published as "important". ||
|| **IMPORTANT_DATE_END** | Specifies the date/time value until which the message will be considered important. ||
|#

{% include [Parameter notes](../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'log.blogpost.add',
    		{
    			POST_TITLE: 'Title',
    			POST_MESSAGE: 'Text',
    			DEST: ['SG1', 'U2']
    		}
    	);
    	
    	const result = response.getData().result;
    	alert('OK!');
    }
    catch( error )
    {
    	console.log(error);
    }
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'log.blogpost.add',
                [
                    'POST_TITLE'   => 'Title',
                    'POST_MESSAGE' => 'Text',
                    'DEST'         => ['SG1', 'U2']
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        alert('OK!');
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding blog post: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('log.blogpost.add', {
        POST_TITLE: 'Title',
        POST_MESSAGE: 'Text',
        DEST: ['SG1', 'U2']
    }, result => {
        if(result.error())
        {
            console.log(result.error());
        }
        else
        {
            alert('OK!');
        }
    });
    ```

{% endlist %}

## Request

{% list tabs %}

- URL request

    ```http
    https://my.bitrix24.com/rest/log.blogpost.add.json?POST_MESSAGE=Hello%2C%20world!&auth=d9a76e2929b7bc1ff21aee9c0ce7e3e2
    ```

{% endlist %}

## Response

```json
{"result": true}
```

## See also

[Using the REST API method log.blogpost.add](http://dev.quickbooks.com/community/blogs/wladart/rest_logblogpostadd.php) (developer blog)