# Add a comment to the News Feed message log.blogcomment.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no success response
- no error response
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: any user

Adds a comment to the specified News Feed message.

#|
|| **Parameter** | **Description** | **Version** ||
|| **USER_ID** | Author of the comment. A user with regular permissions cannot specify another user's ID as a value. This option is only available to users with administrator rights | ||
|| **POST_ID** | ID of the message | ||
|| **TEXT** | Text of the comment | ||
|| **FILES** | Files, an array of values described by the rules of [working with files](../files/how-to-upload-files.md) | ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'log.blogcomment.add',
    		{
    			POST_ID: 10,
    			TEXT: 'Comment on the post'
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
                'log.blogcomment.add',
                [
                    'POST_ID' => 10,
                    'TEXT'    => 'Comment on the post',
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
        echo 'Error adding blog comment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Example of adding
    BX24.callMethod('log.blogcomment.add', {
        POST_ID: 10,
        TEXT: 'Comment on the post'
    }, result => {
        console.log(result);
    });
    ```

{% endlist %}

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'log.blogcomment.user.get',
    		{
    			FIRST_ID: 893,
    			LAST_ID: 894
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	alert("Error: " + error);
    }
    ```

- PHP


    ```php
    try {
        $params = [
            'FIRST_ID' => 893, // id from the b_sonet_log_comment table
            'LAST_ID'  => 894,
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'log.blogcomment.user.get',
                $params
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting blog comments: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Retrieves a comment from the news feed. If no id is passed in the filter, it will return all available comments based on permissions
    let params = {
        FIRST_ID: 893, //id from the b_sonet_log_comment table
        LAST_ID: 894,
    };
    BX24.callMethod('log.blogcomment.user.get', params,
        result => {
            if(result.error())
            {
                alert("Error: " + result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

{% endlist %}

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'log.blogcomment.delete',
    		{
    			COMMENT_ID: 261, //id from the b_blog_comment table
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	alert("Error: " + error);
    }
    ```

- PHP


    ```php
    try {
        $params = [
            'COMMENT_ID' => 261, //id from the b_blog_comment table
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'log.blogcomment.delete',
                $params
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting comment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Deletes a comment from the news feed
    let params = {
        COMMENT_ID: 261, //id from the b_blog_comment table
    };
    BX24.callMethod('log.blogcomment.delete', params,
        result => {
            if(result.error())
            {
                alert("Error: " + result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

{% endlist %}