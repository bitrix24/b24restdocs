# Send a request to join the group sonet_group.user.request

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of an error
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

This method sends a request from the current user to join a social network group, while checking the current user's access rights to the group. If the group is open for free joining, the user immediately becomes a member.

## Returns `true` if the request was successful, or an error message.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | ID of the group to which the request is sent. ||
|| **MESSAGE** | Text of the request. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sonet_group.user.request',
    		{
    			'GROUP_ID': 17,
    			'MESSAGE': 'Request'
    		}
    	);
    	
    	const result = response.getData().result;
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
                'sonet_group.user.request',
                [
                    'GROUP_ID' => 17,
                    'MESSAGE'  => 'Request',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sending request to join group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Sending a request to join the group with ID=17
    BX24.callMethod('sonet_group.user.request', {
        'GROUP_ID': 17,
        'MESSAGE': 'Request'
    });
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Request:

```
https://mydomain.bitrix24.com/rest/sonet_group.user.request.json?auth=52423d4a5f19f5f964f9b4e96a925cfa&GROUP_ID=17&MESSAGE=Request
```

## Response:

>200 OK

```json
{"result":true}
```