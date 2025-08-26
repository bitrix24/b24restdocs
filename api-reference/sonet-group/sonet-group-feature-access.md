# Check the current user's access permissions sonet_group.feature.access

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of error
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

Checks whether the current user has the right to perform an operation in the social network group by calling the function `CSocNetFeaturesPerms::CurrentUserCanPerformOperation()`.

## Request:

```http
https://mydomain.bitrix24.com/rest/sonet_group.feature.access.json?auth=52423d4a5f19f5f964f9b4e96a925cfa&GROUP_ID=1&FEATURE=blog&OPERATION=write_post
```

## Response:

>200 OK

```json
{"result":true}
```

## Parameters

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | ID of the social network group. ||
|| **FEATURE** | Symbolic code of the functionality. ||
|| **OPERATION** | Symbolic code of the operation. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

Returns **true** if the user has the right to perform the operation, **false** if not, and an error in case of incorrect parameters.

{% note info "Note" %}

See the codes of operations and functionalities in the description of the method `CanPerformOperation`.

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sonet_group.feature.access',
    		{
    			'GROUP_ID': 1,
    			'FEATURE': 'blog',
    			'OPERATION': 'write_post'
    		}
    	);
    	
    	const result = response.getData().result;
    	// Your required data processing logic
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
                'sonet_group.feature.access',
                [
                    'GROUP_ID' => 1,
                    'FEATURE' => 'blog',
                    'OPERATION' => 'write_post'
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
        echo 'Error getting group list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Getting the list of the current user's groups

    BX24.callMethod('sonet_group.feature.access', {
        'GROUP_ID': 1,
        'FEATURE': 'blog',
        'OPERATION': 'write_post'
    });
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}