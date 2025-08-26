# Set Full Visibility Settings for Fields rpa.fields.setSettings

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sets the full visibility settings for fields at the stage with the identifier `stageId` of the process with the identifier `typeId`.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **typeId*** 
[`number`](../../../data-types.md) | Identifier of the process ||
|| **stageId** 
[`number`](../../../data-types.md) | Identifier of the stage.

Defaults to `0`, which means — general settings ||
|| **fields*** 
[`object`](../../../data-types.md) | Array with field visibility settings.

If an empty `fields` is passed, all settings will be cleared ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'rpa.comment.add',
    		{
    			"typeId": 1,
    			"fields": {
    				"kanban": [
    					"createdBy",
    					"UF_RPA_1_NAME"
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
                    'typeId' => 1,
                    'fields' => [
                        'kanban' => [
                            'createdBy',
                            'UF_RPA_1_NAME'
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
            "typeId": 1,
            "fields": {
                "kanban": [
                    "createdBy",
                    "UF_RPA_1_NAME"
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

The method will return a result similar to the request [rpa.fields.getSettings](./rpa-fields-get-settings.md).

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-fields-get-settings.md)
- [{#T}](./rpa-fields-set-visibility-settings.md)