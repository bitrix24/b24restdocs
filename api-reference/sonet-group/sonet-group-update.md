# Change social network group parameters sonet_group.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no parameters table
- parameter types not specified
- parameter requirements not specified
- no error response
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method modifies the parameters of a social network group using the API method `CSocNetGroup::Update()`. To perform the operation, the current user must either be the owner of the group or have social network administrator rights.

## Parameters:

It accepts all fields necessary for the `CSocNetGroup::Update()` method, as well as `GROUP_ID` - the ID of the group that needs to be modified.

In case of a successful group modification, it returns its ID; otherwise, it returns an error message.

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sonet_group.update',
    		{
    			'GROUP_ID': 11,
    			'NAME': 'Test sonet group XXX'
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Updated group with ID:', result);
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
                'sonet_group.update',
                [
                    'GROUP_ID' => 11,
                    'NAME' => 'Test sonet group XXX'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating social network group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Changing the name of the social network group with ID=11 to 'Test sonet group XXX'
    BX24.callMethod('sonet_group.update', {
        'GROUP_ID': 11,
        'NAME': 'Test sonet group XXX'
    });
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}

## Request:

```
https://mydomain.bitrix24.com/rest/sonet_group.update.json?auth=803f65e30340ff39703f8061c8b63a10&GROUP_ID=11&NAME=Test%20sonet%20group%20XXX
```

## Response:

>200 OK

```json
{"result":11}
```