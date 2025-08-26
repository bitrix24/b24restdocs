# Change User Role in Group sonet_group.user.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

This method allows changing the role of a user or users in a workgroup. To perform this operation, the current user must have administrator rights in the social network. It is important to note that if the current role of the user is the owner of the group, it cannot be changed using this method.

## Call Parameters

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | ID of the workgroup. ||
|| **USER_ID** | ID of the user (or array of IDs) whose role is being changed. ||
|| **ROLE** | Code of the new role for the group member (available values are `E` - moderator and `K` - participant). ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sonet_group.user.update',
    		{
    			GROUP_ID: 15,
    			USER_ID: [ 10, 21 ],
    			ROLE: 'E'
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Updated user roles:', result);
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
                'sonet_group.user.update',
                [
                    'GROUP_ID' => 15,
                    'USER_ID'  => [10, 21],
                    'ROLE'     => 'E',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating user roles: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Changing roles of users with ID=10 and 21 in the social network group with ID=15 to moderators
    BX24.callMethod('sonet_group.user.update', {
        GROUP_ID: 15,
        USER_ID: [ 10, 21 ],
        ROLE: 'E'
    });
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}