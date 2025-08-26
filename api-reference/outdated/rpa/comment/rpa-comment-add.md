# Create a New Comment in the Timeline rpa.comment.add

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method creates a new comment in the timeline of the item with the identifier `itemId` of the process with the identifier `typeId`.

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`integer`](../../../data-types.md) | Identifier of the process ||
|| **itemId** 
[`integer`](../../../data-types.md) | Identifier of the item ||
|| **fields** 
[`object`](../../../data-types.md) | Object describing the [fields](#fields) of the comment ||
|#

## Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **description** 
[`string`](../../../data-types.md) | Description of the entry. HTML and BB-code formatting can be used ||
|| **files** 
[`array`](../../../data-types.md) | Array of attached files. Each element is an array with the name and content encoded in base64 ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

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
    					[
    						"document.pdf", "...base64_decoded_content..."
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
                                'document.pdf', '...base64_decoded_content...'
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
                    [
                        "document.pdf", "...base64_decoded_content..."
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


## Response Handling

HTTP status: **200**

```json
{
    "comment": {
        "id": 350,
        "createdTime": "2020-03-27T16:00:59+02:00",
        "isFixed": false,
        "typeId": 24,
        "itemId": 10,
        "action": "comment",
        "description": "Mention of user with id 1",
        "userId": 1,
        "title": "Comment",
        "data": {
            "files": [
                15
            ]
        },
        "createdTimestamp": 1585317659000,
        "htmlDescription": "Mention of user with id 1 <a class=\"blog-p-user-name\" id=\"bp_K6r6vvp7\" href=\"/company/personal/user/1/\" bx-tooltip-user-id=\"1\">Anton Gorbylev</a> ",
        "textDescription": "Mention of user with id 1 Anton",
        "users": {
            "1": {
                "id": "1",
                "name": "Anton",
                "secondName": "",
                "lastName": "",
                "title": null,
                "workPosition": "",
                "fullName": "Anton",
                "link": "/company/personal/user/1/"
            }
        }
    }
}
```

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-comment-update.md)
- [{#T}](./rpa-comment-delete.md)