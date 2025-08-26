# Get the list of groups for the current user sonet_group.user.groups

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no parameters and parameter types
- required parameters not specified
- no response in case of error
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns an array of social network groups for the current user by calling `CSocNetUserToGroup::GetList()`.

## Fields of each group:

- **GROUP_ID** - ID of the social network group.
- **GROUP_NAME** - name of the social network group.
- **ROLE** - user's role in the group:
  - **SONET_ROLES_OWNER (A)** - owner,
  - **SONET_ROLES_MODERATOR (E)** - moderator,
  - **SONET_ROLES_USER (K)** - user.

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod('sonet_group.user.groups', {});
    	
    	const result = response.getData().result;
    	console.log('Result:', result);
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
                'sonet_group.user.groups',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting user groups: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Getting the list of groups for the current user
    BX24.callMethod('sonet_group.user.groups', {});
    ```

{% endlist %}

{% include [Examples note](../../_includes/examples.md) %}

## Request:

```
https://mydomain.bitrix24.com/rest/sonet_group.user.groups.json?auth=52423d4a5f19f5f964f9b4e96a925cfa
```

## Response:

>200 OK

```json
{
"result": [
    {"GROUP_ID":"1","GROUP_NAME":"Marketing and Advertising","ROLE":"A"},
    {"GROUP_ID":"3","GROUP_NAME":"Sales","ROLE":"A"},
    {"GROUP_ID":"5","GROUP_NAME":"Leisure","ROLE":"A"},
    {"GROUP_ID":"7","GROUP_NAME":"Technology","ROLE":"A"},
    {"GROUP_ID":"9","GROUP_NAME":"Freelance","ROLE":"A"},
    {"GROUP_ID":"13","GROUP_NAME":"Test sonet group","ROLE":"A"},
    {"GROUP_ID":"15","GROUP_NAME":"Test sonet group","ROLE":"A"}
]
}
```