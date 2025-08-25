# Update Task tasks.task.update

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- An additional example is needed with an explanation on linking a task to CRM
- Parameter types are not specified
- Required parameters are not indicated
- Examples are missing (there should be three examples - curl, js, php)
- Response in case of error is missing
- Response in case of success is missing
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.update` updates a task.

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|| **fields**
[`unknown`](../data-types.md) | Fields corresponding to the available list of fields [tasks.task.getfields](./tasks-task-get-fields.md). ||
|#

## Example

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":11,"fields":{"UF_CRM_TASK":["L_4","C_7","CO_5","D_10"],"UF_TASK_WEBDAV_FILES":["n12345","n67890"],"RESPONSIBLE_ID":123}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":11,"fields":{"UF_CRM_TASK":["L_4","C_7","CO_5","D_10"],"UF_TASK_WEBDAV_FILES":["n12345","n67890"],"RESPONSIBLE_ID":123},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"tasks.task.update",
    		{
    			taskId: 11, // Task identifier you want to update
    			fields: {
    				// Example of passing multiple values in the UF_CRM_TASK field
    				UF_CRM_TASK: [
    					"L_4", // Link to lead with id 4
    					"C_7", // Link to contact with id 7
    					"CO_5", // Link to company with id 5
    					"D_10" // Link to deal with id 10
    				],
    				// Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
    				UF_TASK_WEBDAV_FILES: [
    					"n12345", // Identifier of the first disk file
    					"n67890" // Identifier of the second disk file
    				],
    				RESPONSIBLE_ID: 123 // Identifier of the new assignee
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info("Task successfully updated");
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.update',
                [
                    'taskId' => 11, // Task identifier you want to update
                    'fields' => [
                        // Example of passing multiple values in the UF_CRM_TASK field
                        'UF_CRM_TASK' => [
                            'L_4', // Link to lead with id 4
                            'C_7', // Link to contact with id 7
                            'CO_5', // Link to company with id 5
                            'D_10' // Link to deal with id 10
                        ],
                        // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                        'UF_TASK_WEBDAV_FILES' => [
                            'n12345', // Identifier of the first disk file
                            'n67890' // Identifier of the second disk file
                        ],
                        'RESPONSIBLE_ID' => 123 // Identifier of the new assignee
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Task successfully updated';
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating task: ' . $e->getMessage();
    }
    ```

- BX24.js

   ```javascript
    BX24.callMethod(
        "tasks.task.update",
        {
            taskId: 11, // Task identifier you want to update
            fields: {
                // Example of passing multiple values in the UF_CRM_TASK field
                UF_CRM_TASK: [
                    "L_4", // Link to lead with id 4
                    "C_7", // Link to contact with id 7
                    "CO_5", // Link to company with id 5
                    "D_10" // Link to deal with id 10
                ],
                // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                UF_TASK_WEBDAV_FILES: [
                    "n12345", // Identifier of the first disk file
                    "n67890" // Identifier of the second disk file
                ],
                RESPONSIBLE_ID: 123 // Identifier of the new assignee
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info("Task successfully updated");
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.update',
        [
            'taskId' => 11, // Task identifier you want to update
            'fields' => [
                // Example of passing multiple values in the UF_CRM_TASK field
                'UF_CRM_TASK' => [
                    'L_4', // Link to lead with id 4
                    'C_7', // Link to contact with id 7
                    'CO_5', // Link to company with id 5
                    'D_10' // Link to deal with id 10
                ],
                // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                'UF_TASK_WEBDAV_FILES' => [
                    'n12345', // Identifier of the first disk file
                    'n67890' // Identifier of the second disk file
                ],
                'RESPONSIBLE_ID' => 123 // Identifier of the new assignee
            ]
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Task successfully updated';
    }
    ```

- HTTP

    ```bash
    POST /rest/tasks.task.update.json
    Host: your_bitrix24_domain
    Authorization: Bearer your_access_token
    Content-Type: application/x-www-form-urlencoded

    taskId=11&
    fields[RESPONSIBLE_ID]=123&
    fields[UF_CRM_TASK][0]=L_4&
    fields[UF_CRM_TASK][1]=C_7&
    fields[UF_CRM_TASK][2]=CO_5&
    fields[UF_CRM_TASK][3]=D_10&
    fields[UF_TASK_WEBDAV_FILES][0]=n12345&
    fields[UF_TASK_WEBDAV_FILES][1]=n67890
    ```

{% endlist %}

Method parameters for attaching a file to a task from the disk:

```json
{"taskId": "77", "fields": {"UF_TASK_WEBDAV_FILES": ["n111"]}}
```

where "111" is the file id on the disk.

{% note warning %}

You need to add the letter `n` at the beginning.

{% endnote %}

**Starting from version 22.1300.0**, you can pass the `SE_PARAMETER` parameter to the method - a list of objects with additional task parameters.

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.task.add',
    		{
    			data: {
    				fields: {
    					"TITLE": 'REST',
    					"RESPONSIBLE_ID": 1,
    					"SE_PARAMETER": [
    						{
    							'VALUE': 'Y',
    							'CODE': 3
    						},
    						{
    							'VALUE': 'Y',
    							'CODE': 2
    						},
    					]
    				}
    			}
    		}
    	);
    	
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
                'tasks.task.add',
                [
                    'data' => [
                        'fields' => [
                            'TITLE'          => 'REST',
                            'RESPONSIBLE_ID' => 1,
                            'SE_PARAMETER'   => [
                                [
                                    'VALUE' => 'Y',
                                    'CODE'  => 3
                                ],
                                [
                                    'VALUE' => 'Y',
                                    'CODE'  => 2
                                ],
                            ]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX.ajax.runAction("tasks.task.add", {
        data: {
            fields: {
                "TITLE": 'REST',
                "RESPONSIBLE_ID": 1,
                "SE_PARAMETER": [
                    {
                        'VALUE': 'Y',
                        'CODE': 3
                    },
                    {
                        'VALUE': 'Y',
                        'CODE': 2
                    },
                ]
            }
        }
    }).then(function (response) { console.log(response);});
    ```

{% endlist %}

Code values:

1. deadlines are determined by the deadlines of subtasks
2. automatically complete the task when subtasks are completed (and vice versa)
3. mandatory report upon task completion

{% include [Examples Note](../../_includes/examples.md) %}

## Continue Learning

- [{#T}](../../tutorials/tasks/how-to-upload-file-to-task.md)
- [{#T}](../../tutorials/tasks/how-to-connect-task-to-spa.md)
- [{#T}](../../tutorials/tasks/how-to-create-comment-with-file.md)