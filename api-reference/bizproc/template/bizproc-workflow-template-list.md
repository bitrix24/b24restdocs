# Get the list of templates bizproc.workflow.template.list

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of business process templates.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../../data-types.md) | The array contains a list of [fields](#fields) to select.

You can specify only the fields that are necessary.

Default value — `['ID']` ||
|| **FILTER**
[`object`](../../data-types.md) | An object for filtering the list of business process templates in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — [field](#fields) of the template for filtering
- `value_N` — field value

You can specify the type of filtering before the name of the filtered field:
- `!` — not equal
- `<` — less than
- `<=` — less than or equal to
- `>` — greater than
- `>=` — greater than or equal to | ||
|| **ORDER**
[`object`](../../data-types.md) | An object for sorting the list of launched business processes in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where
- `field_N` — [field](#fields) of the template for sorting
- `value_N` — sorting direction

Sorting direction can take the values:
- `asc` — ascending
- `desc` — descending
  
You can specify multiple fields for sorting, for example, `{NAME: 'ASC', ID: 'DESC'}`.

Default value — `{ID: 'ASC'}` ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` — the number of the desired page ||
|#

### Template Fields {#fields}

#|
|| **Name**
`type` | **Description**||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the business process template ||
|| **MODULE_ID**
[`string`](../../data-types.md) | Identifier of the module by document. Possible values:
- `crm` — CRM
- `lists` — universal lists
- `disk` — drive ||
|| **ENTITY**
[`string`](../../data-types.md) | Identifier of the object by document. Possible values:

CRM
- `CCrmDocumentLead` — leads
- `CCrmDocumentContact` — contacts
- `CCrmDocumentCompany` — companies
- `CCrmDocumentDeal` — deals
- `Bitrix\Crm\Integration\BizProc\Document\Quote` — estimates
- `Bitrix\Crm\Integration\BizProc\Document\SmartInvoice` — invoices
- `Bitrix\Crm\Integration\BizProc\Document\Dynamic` — SPAs

Lists
- `BizprocDocument` — processes in the news feed
- `Bitrix\Lists\BizprocDocumentLists` — lists in groups

Drive
- `Bitrix\Disk\BizProcDocument` ||
|| **DOCUMENT_TYPE**
[`integer`](../../data-types.md) | Document type. Possible values:
crm:
- `LEAD` — leads
- `CONTACT` — contacts
- `COMPANY` — companies
- `DEAL` — deals
- `QUOTE` — estimates
- `SMART_INVOICE` — invoices
- `DYNAMIC_XXX` — SPAs, where XXX — identifier of the SPA

lists:
- `iblock_XXX` — information block, where XXX — identifier of the information block

drive:
- `STORAGE_XXX` — drive storage, where XXX — identifier of the storage
 ||
|| **AUTO_EXECUTE**
[`integer`](../../data-types.md) | Auto-execute flag. Can take values:

- `0` — no auto-execute
- `1` — execute on creation
- `2` — execute on modification
- `3` — execute on creation and modification
||
|| **NAME**
[`string`](../../data-types.md) | Template name ||
|| **TEMPLATE**
[`array`](../../data-types.md) | Array with the description of the template's action structure ||
|| **PARAMETERS**
[`array`](../../data-types.md) | Template parameters ||
|| **VARIABLES**
[`array`](../../data-types.md) | Template variables ||
|| **CONSTANTS**
[`array`](../../data-types.md) | Template constants ||
|| **MODIFIED**
[`datetime`](../../data-types.md) | Date of last modification ||
|| **IS_MODIFIED**
[`boolean`](../../data-types.md) | Whether the template has been modified. Possible values:
- `Y` — yes, it has been modified
- `N` — no

This option is needed for [typical templates](https://helpdesk.bitrix24.com/open/5415841/) of business processes ||
|| **USER_ID**
[`integer`](../../data-types.md) | Identifier of the user who created or modified the template ||
|| **SYSTEM_CODE**
[`string`](../../data-types.md) | System code of the template.

Needed for identifying typical business process templates or templates created by the application ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["ID","NAME","USER_ID","SYSTEM_CODE"],"filter":{"MODULE_ID":"lists","AUTO_EXECUTE":0},"order":{"ID":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.workflow.template.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["ID","NAME","USER_ID","SYSTEM_CODE"],"filter":{"MODULE_ID":"lists","AUTO_EXECUTE":0},"order":{"ID":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.workflow.template.list
    ```

- JS


    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    const parameters = {
        select: [
            'ID',
            'NAME',
            'USER_ID',
            'SYSTEM_CODE'
        ],
        filter: {
            MODULE_ID: 'lists',
            AUTO_EXECUTE: 0
        },
        order: {
            ID: 'DESC'
        }
    };
    
    try {
        const response = await $b24.callListMethod('bizproc.workflow.template.list', parameters);
        const items = response.getData() || [];
        for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
        const generator = $b24.fetchListMethod('bizproc.workflow.template.list', parameters, 'ID');
        for await (const page of generator) {
            for (const entity of page) { console.log('Entity:', entity); }
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
        const response = await $b24.callMethod('bizproc.workflow.template.list', parameters, 0);
        const result = response.getData().result || [];
        for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
        console.error('Request failed', error);
    }
    ```

- PHP

	```php
	try {
		$result = $serviceBuilder
			->getBizProcScope()
			->template()
			->list(
				['ID', 'MODULE_ID', 'ENTITY', 'DOCUMENT_TYPE', 'AUTO_EXECUTE', 'NAME', 'TEMPLATE', 'PARAMETERS', 'VARIABLES', 'CONSTANTS', 'MODIFIED', 'IS_MODIFIED', 'USER_ID', 'SYSTEM_CODE'],
				[]
			);
		foreach ($result->getTemplates() as $template) {
			print("ID: " . $template->ID . "\n");
			print("MODULE_ID: " . $template->MODULE_ID . "\n");
			print("ENTITY: " . $template->ENTITY . "\n");
			print("DOCUMENT_TYPE: " . json_encode($template->DOCUMENT_TYPE) . "\n");
			print("AUTO_EXECUTE: " . ($template->AUTO_EXECUTE ? $template->AUTO_EXECUTE->value : 'null') . "\n");
			print("NAME: " . $template->NAME . "\n");
			print("TEMPLATE: " . json_encode($template->TEMPLATE) . "\n");
			print("PARAMETERS: " . json_encode($template->PARAMETERS) . "\n");
			print("VARIABLES: " . json_encode($template->VARIABLES) . "\n");
			print("CONSTANTS: " . json_encode($template->CONSTANTS) . "\n");
			print("MODIFIED: " . ($template->MODIFIED ? $template->MODIFIED->format(DATE_ATOM) : 'null') . "\n");
			print("IS_MODIFIED: " . ($template->IS_MODIFIED ? 'true' : 'false') . "\n");
			print("USER_ID: " . $template->USER_ID . "\n");
			print("SYSTEM_CODE: " . $template->SYSTEM_CODE . "\n");
			print("\n");
		}
	} catch (Throwable $e) {
		print("Error: " . $e->getMessage() . "\n");
	}
	```

- BX24.js

    ```js
    BX24.callMethod(
        'bizproc.workflow.template.list',
        {
            select: [
                'ID',
                'NAME',
                'USER_ID',
                'SYSTEM_CODE'
            ],
            filter: {
                MODULE_ID: 'lists',
                AUTO_EXECUTE: 0
            },
            order: {
                ID: 'DESC'
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
        'bizproc.workflow.template.list',
        [
            'select' => [
                'ID',
                'NAME',
                'USER_ID',
                'SYSTEM_CODE'
            ],
            'filter' => [
                'MODULE_ID' => 'lists',
                'AUTO_EXECUTE' => 0
            ],
            'order' => [
                'ID' => 'DESC'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "525",
            "NAME": "Display Time",
            "USER_ID": "503",
            "SYSTEM_CODE": "rest_app_5"
        },
        {
           "ID": "379",
           ... 
        }
        ...
    ],
    "total": 34,
    "time": {
        "start": 1737535822.539526,
        "finish": 1737535822.564579,
        "duration": 0.025053024291992188,
        "processing": 0.0019738674163818359,
        "date_start": "2025-01-22T11:50:22+02:00",
        "date_finish": "2025-01-22T11:50:22+02:00",
        "operating_reset_at": 1737536422,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response. 

Contains an array of objects with information about business process templates.

Each object contains [fields](#fields) of the template specified in the `SELECT` parameter ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!",
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ACCESS_DENIED` | Access denied! | The method was not executed by an administrator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-workflow-template-add.md)
- [{#T}](./bizproc-workflow-template-update.md)
- [{#T}](./bizproc-workflow-template-delete.md)
