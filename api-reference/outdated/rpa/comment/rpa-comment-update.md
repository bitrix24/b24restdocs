# Update Timeline Entry rpa.comment.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method updates the timeline entry with the identifier `id`. It only updates the `title` and `description` fields.

The method allows changes only to comments that were added by the same user.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier of the comment ||
|| **fields** 
[`object`](../../../data-types.md) | An object describing the [fields](#fields) of the comment ||
|#

## Fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **description** 
[`integer`](../../../data-types.md) | Description of the entry. HTML and BB-code formatting can be used ||
|| **files** 
[`integer`](../../../data-types.md) | An array of attached files. Each element is an array containing the name and content encoded in base64.

To add a new file, you must provide a list where the key `id` corresponds to the identifier of the file attached to this comment.

For uploading new files, you also need to provide an array with the name and content of the file in base64. ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'rpa.comment.add',
    		{
    			"typeId": 24,
    			"itemId": 10,
    			"fields": {
    				"description": "Mention of user with id 1",
    				"files": [
    					{
    						"id": 15 // identifier of the old file
    					},
    					[
    						"another_document.pdf", "...base64_decoded_content..."
    					]
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('response', result.answer);
    	if(result.error())
    		alert("Error: " + result.error());
    	else
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
                'rpa.comment.add',
                [
                    'typeId' => 24,
                    'itemId' => 10,
                    'fields' => [
                        'description' => 'Mention of user with id 1',
                        'files'      => [
                            [
                                'id' => 15 // identifier of the old file
                            ],
                            [
                                'another_document.pdf', '...base64_decoded_content...'
                            ]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        echo 'response: ' . $result['answer'];
        if ($result['error']) {
            echo 'Error: ' . $result['error'];
        } else {
            echo 'Data: ' . print_r($result['data'], true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding comment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'rpa.comment.add',
        {
            "typeId": 24,
            "itemId": 10,
            "fields": {
                "description": "Mention of user with id 1",
                "files": [
                    {
                        "id": 15 // identifier of the old file
                    },
                    [
                        "another_document.pdf", "...base64_decoded_content..."
                    ]
                ]
            }
        },
        function(result) {
            console.log('response', result.answer);
            if(result.error())
                alert("Error: " + result.error());
            else
            console.log(result.data());
        }
    )
    ```

{% endlist %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-comment-add.md)
- [{#T}](./rpa-comment-delete.md)