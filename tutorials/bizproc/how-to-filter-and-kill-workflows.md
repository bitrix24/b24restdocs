# How to Mass Terminate Workflows with Date Filtering

> Scope: [`bizproc`](../../api-reference/scopes/permissions.md)
> 
> Who can execute methods: administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

During the operation of Bitrix24, you may accumulate stuck workflows or processes that remain in the "In Progress" status for too long and become irrelevant.

To mass terminate old workflows, we will sequentially execute two methods:
1. [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) — retrieve a filtered list of processes
2. [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) — terminate workflows with data deletion. If you need to preserve the fact that the workflow was initiated, use the [bizproc.workflow.terminate](../../api-reference/bizproc/bizproc-workflow-terminate.md) method. Both methods are called in the same way.

## 1. Retrieve the List of Processes {#workflow_id}

We will use the [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) method with a filter:

- `<STARTED` — specify the start date with the prefix `<`, only processes that were started before this date will be selected.

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'bizproc.workflow.instances',
        {
            filter: {
                '<STARTED': '2025-01-01T00:00:00Z'
            }
        },
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.workflow.instances',
        [
            'filter' => [
                '<STARTED' => '2025-01-01T00:00:00Z'
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will obtain the `ID` of all active workflows that were initiated before the specified date.

```json
{
    "result": [
        {
            "ID": "660e559f34af10.95144732",
            "MODIFIED": "2024-12-04T10:04:24+03:00",
            "OWNED_UNTIL": null
        },
        {
            "ID": "6639c7b59e9eb5.40607056",
            "MODIFIED": "2024-12-04T09:52:40+03:00",
            "OWNED_UNTIL": null
        },
        {
            "ID": "66ea9200131729.26195442",
            "MODIFIED": "2024-09-18T11:42:28+03:00",
            "OWNED_UNTIL": null
        },
        {
            "ID": "65ef0868368978.47049110",
            "MODIFIED": "2024-03-11T16:34:32+03:00",
            "OWNED_UNTIL": null
        }
    ],
    "total": 4
}
```

## 2. Terminate Workflows 

We will use the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method with the parameter:
- `ID` — the identifier of the process, we pass the `ID` obtained in [step 1](#workflow_id).

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        'bizproc.workflow.kill',
        {
            ID: '660e559f34af10.95144732',
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.workflow.kill',
        [
            'ID' => '660e559f34af10.95144732'
        ]
    );
    ```

{% endlist %}

As a result, we will receive `true`, indicating that the process was successfully deleted. If you encounter an `error`, refer to the documentation for possible errors in the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method.

```json
{
    "result": true
}
```

## Code Example

In this example, all found processes are deleted in a loop. When deleting a large volume of data, there may be limitations on request execution. To optimize the code for your workload, refer to the [Performance](../../settings/performance/index.md) section.

{% list tabs %}

- JS
  
    ```javascript  
    // Function to convert date from dd.mm.yyyy format to ISO format
    function convertDateToISO(dateString) {
        const [day, month, year] = dateString.split('.');
        return `${year}-${month}-${day}T00:00:00Z`;
    }

    // Request date from user
    const userDateInput = prompt("Enter the date in dd.mm.yyyy format:");
    const isoDate = convertDateToISO(userDateInput);

    // Call the bizproc.workflow.instances method with date filter
    BX24.callMethod(
        'bizproc.workflow.instances',
        {
            filter: {
                '<STARTED': isoDate
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                const instances = result.data();
                instances.forEach(instance => {
                    const instanceId = instance.ID;
                    
                    // Call the bizproc.workflow.kill method for each ID
                    BX24.callMethod(
                        'bizproc.workflow.kill',
                        {
                            ID: instanceId
                        },
                        function(killResult) {
                            if (killResult.error()) {
                                console.error(`Error deleting process ${instanceId}:`, killResult.error());
                            } else {
                                console.log(`Process ${instanceId} successfully deleted.`);
                            }
                        }
                    );
                });

                if (result.more()) {
                    result.next();
                }
            }
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    // Function to convert date from dd.mm.yyyy format to ISO format
    function convertDateToISO($dateString) {
        list($day, $month, $year) = explode('.', $dateString);
        return "{$year}-{$month}-{$day}T00:00:00Z";
    }

    // Request date from user
    $userDateInput = readline("Enter the date in dd.mm.yyyy format: ");
    $isoDate = convertDateToISO($userDateInput);

    // Call the bizproc.workflow.instances method with date filter
    $result = CRest::call(
        'bizproc.workflow.instances',
        [
            'filter' => [
                '<STARTED' => $isoDate
            ]
        ]
    );

    if (!empty($result['error'])) {
        echo "Error: " . $result['error_description'];
    } else {
        $instances = $result['result'];
        foreach ($instances as $instance) {
            $instanceId = $instance['ID'];

            // Call the bizproc.workflow.kill method for each ID
            $killResult = CRest::call(
                'bizproc.workflow.kill',
                [
                    'ID' => $instanceId
                ]
            );

            if (!empty($killResult['error'])) {
                echo "Error deleting process {$instanceId}: " . $killResult['error_description'] . "\n";
            } else {
                echo "Process {$instanceId} successfully deleted.\n";
            }
        }

        // Check for additional data
        while (!empty($result['next'])) {
            $result = CRest::call(
                'bizproc.workflow.instances',
                [
                    'filter' => [
                        '<STARTED' => $isoDate
                    ],
                    'start' => $result['next']
                ]
            );

            $instances = $result['result'];
            foreach ($instances as $instance) {
                $instanceId = $instance['ID'];

                // Call the bizproc.workflow.kill method for each ID
                $killResult = CRest::call(
                    'bizproc.workflow.kill',
                    [
                        'ID' => $instanceId
                    ]
                );

                if (!empty($killResult['error'])) {
                    echo "Error deleting process {$instanceId}: " . $killResult['error_description'] . "\n";
                } else {
                    echo "Process {$instanceId} successfully deleted.\n";
                }
            }
        }
    }
    ```

{% endlist %}