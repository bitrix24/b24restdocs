# How to Complete Business Processes of a Terminated Employee

> Scope: [`user_brief, user_basic, user, bizproc`](../../api-reference/scopes/permissions.md)
> 
> Who can execute methods: administrator

When an employee is terminated in Bitrix24, there may be unfinished business processes for which they were responsible.

To complete the active business processes of the terminated employee, we will sequentially execute three methods:

1. [user.get](../../api-reference/user/user-get.md) — retrieve the `ID` of the terminated employee

2. [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) — get the list of tasks for the processes that the terminated employee was responsible for

3. [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) — complete the business processes with data deletion. If you need to preserve the fact that the business process was initiated, use the method [bizproc.workflow.terminate](../../api-reference/bizproc/bizproc-workflow-terminate.md). Both methods are called in the same way.

## 1. Retrieve the ID of the Terminated Employee {#user-id}

We will use the method [user.get](../../api-reference/user/user-get.md) with the following filter:

- `NAME` — specify the employee's first name

- `LAST_NAME` — specify the employee's last name

- `ACTIVE` — this parameter controls the search for active or terminated employees. If this parameter is not passed, the search will include all employees regardless of their status. We will specify `0` to search only among terminated employees.

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        "user.get",
        {
            filter: {
                "NAME": "employee's name",
                "LAST_NAME": "employee's last name",
                "ACTIVE": 0
            }
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.get',
        [
            'filter' => [
                'NAME' => "employee's name",
                'LAST_NAME' => "employee's last name",
                'ACTIVE' => 0
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will obtain the `ID` of the terminated employee.

```json
{
    "result": [
        {
            "ID": "29",
            "XML_ID": "28936832",
            "ACTIVE": false,
            "NAME": "employee's name",
            "LAST_NAME": "employee's last name",
            "SECOND_NAME": "",
            "TITLE": "",
            "EMAIL": "employee_email@gmail.com",
            "LAST_LOGIN": "2025-03-27T13:49:36+02:00",
            "DATE_REGISTER": "2020-04-23T03:00:00+02:00",
            "TIME_ZONE": "Europe/Berlin",
            "IS_ONLINE": "N",
            "TIMESTAMP_X": {},
            "LAST_ACTIVITY_DATE": {},
            "PERSONAL_GENDER": "",
            "PERSONAL_PROFESSION": "",
            "PERSONAL_WWW": "",
            "PERSONAL_BIRTHDAY": "",
            "PERSONAL_PHOTO": "https://cdn-com.bitrix24.com/b13743910/main/3f2/3f212fjf8c3627cfe51633f959de/avatar.png",
            "PERSONAL_ICQ": "",
            "PERSONAL_PHONE": "",
            "PERSONAL_FAX": "",
            "PERSONAL_MOBILE": "",
            "PERSONAL_PAGER": "",
            "PERSONAL_STREET": "",
            "PERSONAL_CITY": "",
            "PERSONAL_STATE": "",
            "PERSONAL_ZIP": "",
            "PERSONAL_COUNTRY": "0",
            "PERSONAL_MAILBOX": "",
            "PERSONAL_NOTES": "",
            "WORK_PHONE": "",
            "WORK_COMPANY": "",
            "WORK_POSITION": "Manager",
            "WORK_DEPARTMENT": "",
            "WORK_WWW": "",
            "WORK_FAX": "",
            "WORK_PAGER": "",
            "WORK_STREET": "",
            "WORK_MAILBOX": "",
            "WORK_CITY": "",
            "WORK_STATE": "",
            "WORK_ZIP": "",
            "WORK_COUNTRY": "0",
            "WORK_PROFILE": "",
            "WORK_NOTES": "",
            "UF_EMPLOYMENT_DATE": "",
            "UF_DEPARTMENT": [
                7,
                1
            ],
            "UF_PHONE_INNER": "555",
            "UF_USR_1619099890455": "12132132123",
            "USER_TYPE": "employee"
        }
    ],
    "total": 1,
}
```

## 2. Retrieve the List of Tasks for the Processes that the Terminated Employee Was Responsible For {#workflow_id}

We will use the method [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) with the following filter:

- `USER_ID` — the identifier of the employee, we will pass the ID obtained in [step 1](#user-id)

- `STATUS` — this parameter indicates the status of the tasks, we will specify `0` to filter only incomplete tasks.

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'bizproc.task.list',
        {
            filter: {
                'USER_ID': 29,
                'STATUS': 0,
            }
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.task.list',
        [
            'filter' => [
                'USER_ID' => 29,
                'STATUS' => 0
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will obtain a list of incomplete tasks. Each task has a `WORKFLOW_ID` parameter — this is the `ID` of the business process that we will complete in the next step.

```json
{
    "result": [
        {
            "ENTITY": "CCrmDocumentContact",
            "DOCUMENT_ID": "CONTACT_2437",
            "ID": "879",
            "WORKFLOW_ID": "67e3db8e581121.72266518",
            "DOCUMENT_NAME": "widget contact",
            "NAME": "Address",
            "DOCUMENT_URL": "/crm/contact/details/2437/"
        },
        {
            "ENTITY": "CCrmDocumentContact",
            "DOCUMENT_ID": "CONTACT_2435",
            "ID": "877",
            "WORKFLOW_ID": "67c5b492d0b426.74280093",
            "DOCUMENT_NAME": "Contact #2435",
            "NAME": "Address",
            "DOCUMENT_URL": "/crm/contact/details/2435/"
        },
        {
            "ENTITY": "CCrmDocumentContact",
            "DOCUMENT_ID": "CONTACT_2433",
            "ID": "875",
            "WORKFLOW_ID": "67c598a987d387.85575151",
            "DOCUMENT_NAME": "Contact #2433",
            "NAME": "Address",
            "DOCUMENT_URL": "/crm/contact/details/2433/"
        },
        {
            "ENTITY": "CCrmDocumentContact",
            "DOCUMENT_ID": "CONTACT_2429",
            "ID": "871",
            "WORKFLOW_ID": "67091df4b13dd2.83077613",
            "DOCUMENT_NAME": "Petrov Vasily",
            "NAME": "Address",
            "DOCUMENT_URL": "/crm/contact/details/2429/"
        },
        {
            "ENTITY": "CCrmDocumentContact",
            "DOCUMENT_ID": "CONTACT_2427",
            "ID": "859",
            "WORKFLOW_ID": "66e2d5d5c64f82.28057011",
            "DOCUMENT_NAME": "Ivanovich",
            "NAME": "Address",
            "DOCUMENT_URL": "/crm/contact/details/2427/"
        },
        {
            "ENTITY": "CCrmDocumentContact",
            "DOCUMENT_ID": "CONTACT_2425",
            "ID": "857",
            "WORKFLOW_ID": "66e0242399d303.52288141",
            "DOCUMENT_NAME": "Petrovna",
            "NAME": "Address",
            "DOCUMENT_URL": "/crm/contact/details/2425/"
        },
        {
            "ENTITY": "CCrmDocumentContact",
            "DOCUMENT_ID": "CONTACT_2423",
            "ID": "855",
            "WORKFLOW_ID": "66d870dfbb9542.91956540",
            "DOCUMENT_NAME": "Smirnov",
            "NAME": "Address",
            "DOCUMENT_URL": "/crm/contact/details/2423/"
        },
        {
            "ENTITY": "CCrmDocumentContact",
            "DOCUMENT_ID": "CONTACT_2421",
            "ID": "853",
            "WORKFLOW_ID": "66d7fb6f86c0c2.49741539",
            "DOCUMENT_NAME": "Kalashnikov",
            "NAME": "Address",
            "DOCUMENT_URL": "/crm/contact/details/2421/"
        },
        {
            "ENTITY": "CCrmDocumentContact",
            "DOCUMENT_ID": "CONTACT_2419",
            "ID": "851",
            "WORKFLOW_ID": "66d073d9c9fc08.23457428",
            "DOCUMENT_NAME": "Unnamed",
            "NAME": "Address",
            "DOCUMENT_URL": "/crm/contact/details/2419/"
        }
    ],
    "total": 9,
}
```

## 3. Complete the Business Processes

We will use the method [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) with the following parameter:

- `ID` — the identifier of the process, we will pass the `WORKFLOW_ID` obtained in [step 2](#workflow_id)
  
{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'bizproc.workflow.kill',
        {
            ID: '67e3db8e581121.72266518',
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.workflow.kill',
        [
            'ID' => '67e3db8e581121.72266518'
        ]
    );
    ```

{% endlist %}

As a result, we will receive `true`, indicating that the process deletion was successful. If you receive an `error`, review the possible error descriptions in the documentation for the method [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md).

```json
{
    "result": true,
}
```

## Code Example

In the example, all found processes are deleted in a loop. If you need to delete a large volume of data, you may encounter limits on request execution. To optimize the code for your workload, use the recommendations in the [Performance](../../settings/performance/index.md) section.

{% list tabs %}

- JS

    ```javascript
    // Function to get employee ID by first name and last name
    function getUserId(firstName, lastName, callback) {
        BX24.callMethod(
            "user.get",
            {
                "NAME": firstName,
                "LAST_NAME": lastName,
                "ACTIVE": 0,
            },
            function(result) {
                if (result.error()) {
                    console.error(result.error());
                } else {
                    // Assuming only one user is found
                    const userId = result.data()[0].ID;
                    callback(userId);
                }
            }
        );
    }

    // Function to get the list of incomplete tasks for the employee
    function getUserTasks(userId, callback) {
        BX24.callMethod(
            'bizproc.task.list',
            {
                filter: {
                    'USER_ID': userId,
                    'STATUS': 0,
                }
            },
            function(result) {
                if (result.error()) {
                    console.error(result.error());
                } else {
                    // Extract WORKFLOW_ID from each task
                    const workflowIds = result.data().map(task => task.WORKFLOW_ID);
                    callback(workflowIds);
                }
            }
        );
    }

    // Function to complete business processes by the list of WORKFLOW_ID
    function killWorkflows(workflowIds) {
        workflowIds.forEach(workflowId => {
            BX24.callMethod(
                'bizproc.workflow.kill',
                {
                    ID: workflowId,
                },
                function(result) {
                    if (result.error()) {
                        console.error(result.error());
                    } else {
                        console.log(`Workflow ${workflowId} completed successfully.`);
                    }
                }
            );
        });
    }

    // Main function that combines all steps
    function processEmployeeTasks(firstName, lastName) {
        getUserId(firstName, lastName, function(userId) {
            getUserTasks(userId, function(workflowIds) {
                killWorkflows(workflowIds);
            });
        });
    }

    // Prompt user for first name and last name
    const firstName = prompt("Enter the employee's first name:");
    const lastName = prompt("Enter the employee's last name:");

    // Start the process
    processEmployeeTasks(firstName, lastName);
    ```

- PHP

    ```php
    require_once('crest.php');

    // Function to get employee ID by first name and last name
    function getUserId($firstName, $lastName) {
        $result = CRest::call(
            'user.get',
            [
                'filter' => [
                    'NAME' => $firstName,
                    'LAST_NAME' => $lastName,
                    'ACTIVE' => 0
                ]
            ]
        );

        if (!empty($result['error'])) {
            echo "Error: " . $result['error_description'];
            return null;
        }

        // Assuming only one user is found
        return $result['result'][0]['ID'] ?? null;
    }

    // Function to get the list of incomplete tasks for the employee
    function getUserTasks($userId) {
        $result = CRest::call(
            'bizproc.task.list',
            [
                'filter' => [
                    'USER_ID' => $userId,
                    'STATUS' => 0
                ]
            ]
        );

        if (!empty($result['error'])) {
            echo "Error: " . $result['error_description'];
            return [];
        }

        // Extract WORKFLOW_ID from each task
        return array_map(function($task) {
            return $task['WORKFLOW_ID'];
        }, $result['result']);
    }

    // Function to complete business processes by the list of WORKFLOW_ID
    function killWorkflows($workflowIds) {
        foreach ($workflowIds as $workflowId) {
            $result = CRest::call(
                'bizproc.workflow.kill',
                [
                    'ID' => $workflowId
                ]
            );

            if (!empty($result['error'])) {
                echo "Error: " . $result['error_description'];
            } else {
                echo "Workflow $workflowId completed successfully.\n";
            }
        }
    }

    // Main function that combines all steps
    function processEmployeeTasks($firstName, $lastName) {
        $userId = getUserId($firstName, $lastName);
        if ($userId) {
            $workflowIds = getUserTasks($userId);
            killWorkflows($workflowIds);
        }
    }

    // Prompt user for first name and last name
    $firstName = readline("Enter the employee's first name: ");
    $lastName = readline("Enter the employee's last name: ");

    // Start the process
    processEmployeeTasks($firstName, $lastName);
    ```

{% endlist %}