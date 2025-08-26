# Remove users from group sonet_group.user.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no response in case of error
- no response in case of success
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

This method allows you to remove a user or users from a workgroup. To perform this operation, the current user must have administrator rights in the social network. It is important to note that if the current user's role is the owner of the group, they cannot be removed from the group using this method.

## Call Parameters

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | ID of the workgroup. ||
|| **USER_ID** | ID of the user (or array of IDs) to be removed from the group. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sonet_group.user.delete',
    		{
    			GROUP_ID: 15,
    			USER_ID: [ 10, 21 ]
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Deleted users from group:', result);
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
                'sonet_group.user.delete',
                [
                    'GROUP_ID' => 15,
                    'USER_ID' => [10, 21]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting users from social network group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Removing users with ID=10 and 21 from the social network group with ID=15
    BX24.callMethod('sonet_group.user.delete', {
        GROUP_ID: 15,
        USER_ID: [ 10, 21 ]
    });
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}