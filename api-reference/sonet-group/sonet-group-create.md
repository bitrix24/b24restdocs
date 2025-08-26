# Create Social Network Group sonet_group.create

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no error response is provided
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

Creates a social network group using the API method `CSocNetGroup::CreateGroup()`, specifying the current user as the group owner.

## Request:

```http
https://mydomain.bitrix24.com/rest/sonet_group.create.json?auth=803f65e30340ff39703f8061c8b63a10&NAME=Test%20sonet%20group&VISIBLE=Y&OPENED=N&INITIATE_PERMS=K
```

## Response:

> 200 OK

```json
{"result":11}
```

## Parameters

#|
|| **Parameter** | **Description** ||
|| **arFields** | Array of parameters for the new group. Allowed keys in the array:
**NAME** - group name (required field),
**DESCRIPTION** - group description,
**VISIBLE** - flag Y/N - whether the group is visible in the group list,
**OPENED** - flag Y/N - whether the group is open for free membership,
**KEYWORDS** - keywords,
**INITIATE_PERMS** - who has the right to invite users to the group (required field):
- **A** - only the group owner,
- **E** - the group owner and group moderators,
- **K** - all group members.
**CLOSED** - flag Y/N - whether the group is archived,
**SPAM_PERMS** - who has the right to send messages to the group (required field). Values are similar to the INITIATE_PERMS parameter.
**PROJECT** - flag Y/N - whether the group is a project or not. By default - it is not. (Since version 18.0.0)<br>**PROJECT_DATE_FINISH** - sets the project end date. (Since version 18.0.0)
**PROJECT_DATE_START** - sets the project start date. (Since version 18.0.0)
**SCRUM_MASTER_ID** - if filled with a user ID, this project will become a scrum. (Since version 22.300) ||
|| **bAutoSubscribe** | Auto-subscription to the created topic. Optional parameter. Defaults to True. (Since version 10.0.0) ||
|#

{% include [Notes on parameters](../../_includes/required.md) %}

In case of successful group creation, it returns its ID; otherwise, it returns an error message.

{% note info "" %}

**Note**: Creating extranet groups via the REST API is not yet possible.

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod('sonet_group.create', {
    		'NAME': 'Test sonet group',
    		'VISIBLE': 'Y',
    		'OPENED': 'N',
    		'INITIATE_PERMS': 'K'
    	});
    
    	const result = response.getData().result;
    	console.log('Created social network group with ID:', result);
    	// Your logic for processing data
    	processResult(result);
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
                'sonet_group.create',
                [
                    'NAME'          => 'Test sonet group',
                    'VISIBLE'       => 'Y',
                    'OPENED'        => 'N',
                    'INITIATE_PERMS' => 'K',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating sonet group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // Create a visible and open social network group named 'Test sonet group' with the right to invite new members for all current group members

    BX24.callMethod('sonet_group.create', {
        'NAME': 'Test sonet group',
        'VISIBLE': 'Y',
        'OPENED': 'N',
        'INITIATE_PERMS': 'K'
    });
    ```

{% endlist %}

{% include [Notes on examples](../../_includes/examples.md) %}