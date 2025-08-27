# Get a List of Running Workflows bizproc.workflow.instances

> Scope: [`bizproc`](../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of running workflows.

{% note info "bizproc.workflow.instance.list" %}

There is an older method `bizproc.workflow.instance.list` — an alias for the method `bizproc.workflow.instances`. It accepts the same parameters and returns the same results.

Support for `bizproc.workflow.instance.list` is not guaranteed in the future, so we recommend using `bizproc.workflow.instances`.

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../data-types.md) | An array containing the list of fields to select.

You can specify only the fields that are necessary.

Available fields:
- `ID` — identifier of the workflow
- `MODIFIED` — date of the last modification
- `OWNED_UNTIL` — time the workflow is locked. The process is considered stuck if the difference between the lock time and the current time is more than 5 minutes
- `MODULE_ID` — module identifier by document
- `ENTITY` — entity identifier by document
- `DOCUMENT_ID` — document identifier
- `STARTED` — date the workflow was started
- `STARTED_BY` — who started the workflow
- `TEMPLATE_ID` — identifier of the workflow template

Default value: `['ID', 'MODIFIED', 'OWNED_UNTIL']` ||
|| **FILTER**
[`object`](../data-types.md) | An object for filtering the list of running workflows in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

The list of filterable fields is the same as for the `SELECT` parameter.

You can specify the type of filtering before the name of the filtered field:
- `!` — not equal
- `<` — less than
- `<=` — less than or equal to
- `>` — greater than
- `>=` — greater than or equal to | ||
|| **ORDER**
[`object`](../data-types.md) | An object for sorting the list of running workflows in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

The list of fields for sorting is the same as for the `SELECT` parameter.

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending
  
Default value: `{'MODIFIED': 'desc'}` ||
|| **start**
[`integer`](../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["ID","MODIFIED","OWNED_UNTIL","MODULE_ID","ENTITY","DOCUMENT_ID","STARTED","STARTED_BY","TEMPLATE_ID"],"order":{"STARTED":"DESC"},"filter":{">STARTED_BY":0}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.workflow.instances
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["ID","MODIFIED","OWNED_UNTIL","MODULE_ID","ENTITY","DOCUMENT_ID","STARTED","STARTED_BY","TEMPLATE_ID"],"order":{"STARTED":"DESC"},"filter":{">STARTED_BY":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.workflow.instances
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'bizproc.workflow.instances',
    		{
    			select: [
    				'ID',
    				'MODIFIED',
    				'OWNED_UNTIL',
    				'MODULE_ID',
    				'ENTITY',
    				'DOCUMENT_ID',
    				'STARTED',
    				'STARTED_BY',
    				'TEMPLATE_ID'
    			],
    			order: {
    				STARTED: 'DESC'
    			},
    			filter: {
    				'>STARTED_BY': 0
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	alert("Error: " + error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'bizproc.workflow.instances',
                [
                    'select' => [
                        'ID',
                        'MODIFIED',
                        'OWNED_UNTIL',
                        'MODULE_ID',
                        'ENTITY',
                        'DOCUMENT_ID',
                        'STARTED',
                        'STARTED_BY',
                        'TEMPLATE_ID'
                    ],
                    'order' => [
                        'STARTED' => 'DESC'
                    ],
                    'filter' => [
                        '>STARTED_BY' => 0
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching workflow instances: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'bizproc.workflow.instances',
        {
            select: [
                'ID',
                'MODIFIED',
                'OWNED_UNTIL',
                'MODULE_ID',
                'ENTITY',
                'DOCUMENT_ID',
                'STARTED',
                'STARTED_BY',
                'TEMPLATE_ID'
            ],
            order: {
                STARTED: 'DESC'
            },
            filter: {
                '>STARTED_BY': 0
            }
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.workflow.instances',
        [
            'select' => [
                'ID',
                'MODIFIED',
                'OWNED_UNTIL',
                'MODULE_ID',
                'ENTITY',
                'DOCUMENT_ID',
                'STARTED',
                'STARTED_BY',
                'TEMPLATE_ID'
            ],
            'order' => [
                'STARTED' => 'DESC'
            ],
            'filter' => [
                '>STARTED_BY' => 0
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result":[
        {
            "DOCUMENT_ID": "LEAD_1",
            "ENTITY": "CCrmDocumentLead",
            "ID": "66e412fdc9bd44.36306599",
            "STARTED": "2024-09-13T10:25:01+00:00",
            "MODULE_ID": "crm",
            "OWNED_UNTIL": null,
            "TEMPLATE_ID": "1",
            "STARTED_BY": "0"
        },
        {
            "DOCUMENT_ID":"DEAL_1633",
            "ENTITY":"CCrmDocumentDeal",
            "ID":"658c4d3d6a2906.51542462",
            "STARTED":"2023-12-27T19:13:49+03:00",
            "MODULE_ID":"crm",
            "OWNED_UNTIL":null,
            "TEMPLATE_ID":"212",
            "STARTED_BY":"57"
        }
    ],
    "total": 2,
    "time": {
        "start": 1726476060.581428,
        "finish": 1726476060.813776,
        "duration": 0.23234796524047852,
        "processing": 0.002630949020385742,
        "date_start": "2024-09-16T08:41:00+00:00",
        "date_finish": "2024-09-16T08:41:00+00:00",
        "operating_reset_at": 1726476660,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. 

Contains an array of objects with information about the running workflows ||
|| **total**
[`integer`](../data-types.md) | The total number of records found ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** |**Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! | Method was not executed by an administrator ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-workflow-start.md)
- [{#T}](./bizproc-workflow-terminate.md)
- [{#T}](./bizproc-workflow-kill.md)
- [{#T}](../../tutorials/bizproc/how-to-kill-workflows.md)
- [{#T}](../../tutorials/bizproc/how-to-filter-and-kill-workflows.md)