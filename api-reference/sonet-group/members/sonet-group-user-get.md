# Get the list of group members sonet_group.user.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of error
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

The method returns an array of social network group members by calling `CSocNetUserToGroup::GetList()`, while checking the current user's access rights to the group. The method does not return inactive users (terminated employees).

## Member fields:

- **USER_ID** - User ID.
- **ROLE** - User role in the group:
  - **SONET_ROLES_OWNER (A)** - owner,
  - **SONET_ROLES_MODERATOR (E)** - moderator,
  - **SONET_ROLES_USER (K)** - user.

## Function parameters

#|
|| **Parameter** | **Description** ||
|| **ID** | ID of the group whose members need to be retrieved. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod('sonet_group.user.get', {
    		'ID': 15
    	});
    	
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
                'sonet_group.user.get',
                [
                    'ID' => 15
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting group users: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Getting the list of social network group members with ID=15
    BX24.callMethod('sonet_group.user.get', {
        'ID': 15
    });
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Request:

```
https://mydomain.bitrix24.com/rest/sonet_group.user.get.json?auth=67df5afc8ce59732e4a21ed3e336979f&ID=15
```

## Response:

>200 OK

```json
{"result":[{"USER_ID":"1","ROLE":"A"}]}
```