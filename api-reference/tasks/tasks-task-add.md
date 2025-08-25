# Add Task tasks.task.add

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- An additional example is needed with an explanation on linking the task to CRM
- Parameter types are not specified
- Parameter requirements are not indicated
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

The method `tasks.task.add` creates a task. 

#|
|| **Parameter** / **Type** | **Description** ||
|| **fields**
[`unknown`](../data-types.md) | Fields corresponding to the available list of fields [tasks.task.getfields](./tasks-task-get-fields.md). ||
|#

## Examples

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"Task Title","DEADLINE":"2023-12-31T23:59:59","CREATED_BY":456,"RESPONSIBLE_ID":123,"UF_CRM_TASK":["L_4","C_7","CO_5","D_10"],"UF_TASK_WEBDAV_FILES":["n12345","n67890"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"Task Title","DEADLINE":"2023-12-31T23:59:59","CREATED_BY":456,"RESPONSIBLE_ID":123,"UF_CRM_TASK":["L_4","C_7","CO_5","D_10"],"UF_TASK_WEBDAV_FILES":["n12345","n67890"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"tasks.task.add",
    		{
    			fields: {               
    				TITLE: "Task Title", // Task Title
    				DEADLINE: "2023-12-31T23:59:59", // Deadline
    				CREATED_BY: 456, // Creator ID
    				RESPONSIBLE_ID: 123, // Assignee ID
    				// Example of passing multiple values in the UF_CRM_TASK field
    				UF_CRM_TASK: [
    					"L_4", // Link to lead
    					"C_7", // Link to contact
    					"CO_5", // Link to company
    					"D_10" // Link to deal
    				],
    				// Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
    				UF_TASK_WEBDAV_FILES: [
    					"n12345", // Identifier of the first Drive file
    					"n67890" // Identifier of the second Drive file
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info("Task successfully created with ID " + result.task.id);
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
                'tasks.task.add',
                [
                    'fields' => [
                        'TITLE'         => 'Task Title',
                        'DEADLINE'      => '2023-12-31T23:59:59',
                        'CREATED_BY'    => 456,
                        'RESPONSIBLE_ID' => 123,
                        'UF_CRM_TASK'   => [
                            'L_4',
                            'C_7',
                            'CO_5',
                            'D_10',
                        ],
                        'UF_TASK_WEBDAV_FILES' => [
                            'n12345',
                            'n67890',
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Task successfully created with ID ' . $result['task']['id'];
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "tasks.task.add",
        {
            fields: {               
                TITLE: "Task Title", // Task Title
                DEADLINE: "2023-12-31T23:59:59", // Deadline
                CREATED_BY: 456, // Creator ID
                RESPONSIBLE_ID: 123, // Assignee ID
                // Example of passing multiple values in the UF_CRM_TASK field
                UF_CRM_TASK: [
                    "L_4", // Link to lead
                    "C_7", // Link to contact
                    "CO_5", // Link to company
                    "D_10" // Link to deal
                ],
                // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                UF_TASK_WEBDAV_FILES: [
                    "n12345", // Identifier of the first Drive file
                    "n67890" // Identifier of the second Drive file
                ]
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info("Task successfully created with ID " + result.data().task.id);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.add',
        [
            'fields' => [
                'TITLE' => 'Task Title', // Task Title
                'DEADLINE' => '2023-12-31T23:59:59', // Deadline
                'CREATED_BY' => 456, // Creator ID
                'RESPONSIBLE_ID' => 123, // Assignee ID
                // Example of passing multiple values in the UF_CRM_TASK field
                'UF_CRM_TASK' => [
                    'L_4', // Link to lead
                    'C_7', // Link to contact
                    'CO_5', // Link to company
                    'D_10' // Link to deal
                ],
                // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                'UF_TASK_WEBDAV_FILES' => [
                    'n12345', // Identifier of the first Drive file
                    'n67890' // Identifier of the second Drive file
                ]
            ]
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Task successfully created with ID ' . $result['result']['task']['id'];
    }
    ```

- HTTP

    ```bash
    POST /rest/tasks.task.add.json
    Host: your_bitrix24_domain
    Authorization: Bearer your_access_token
    Content-Type: application/x-www-form-urlencoded

    fields[TITLE]=Task Title&
    fields[DEADLINE]=2023-12-31T23:59:59&
    fields[CREATED_BY]=456&
    fields[RESPONSIBLE_ID]=123&
    fields[UF_CRM_TASK][0]=L_4&
    fields[UF_CRM_TASK][1]=C_7&
    fields[UF_CRM_TASK][2]=CO_5&
    fields[UF_CRM_TASK][3]=D_10&
    fields[UF_TASK_WEBDAV_FILES][0]=n12345&
    fields[UF_TASK_WEBDAV_FILES][1]=n67890
    ```

{% endlist %}

To attach a file to the task, the file identifier must be prefixed with the character `n`

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'task.item.update',
    		{
    			taskId: '76',
    			fields: {
    				UF_TASK_WEBDAV_FILES: [
    					'n96'
    				]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Updated task with ID:', result);
    	// Your required data processing logic
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
                'tasks.task.update',
                [
                    'taskId' => '76',
                    'fields' => [
                        'UF_TASK_WEBDAV_FILES' => [
                            'n96'
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
        echo 'Error updating task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    {
        "taskId":"76",
        "fields": {
            "UF_TASK_WEBDAV_FILES": [
                "n96"
            ]
        }
    }
    ```

{% endlist %}

**Starting from version 22.1300.0** you can pass the `SE_PARAMETER` parameter to the method — a list of objects with additional task parameters.

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
                                },
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

- [{#T}](../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../tutorials/tasks/how-to-connect-task-to-spa.md)
- [{#T}](../../tutorials/tasks/how-to-create-comment-with-file.md)