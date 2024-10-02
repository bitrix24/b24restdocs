# Start Business Process bizproc.workflow.start

> Scope: [`bizproc`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method initiates a new business process.

You can only start a business process using REST on paid plans, demo licenses, and NFR licenses.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TEMPLATE_ID***
[`integer`](../data-types.md) | Identifier of the business process template ||
|| **DOCUMENT_ID***
[`array`](../data-types.md) | Identifier of the document to start the business process in the format [`module`, `object`, `ELEMENT_ID`].

Examples of entries for different document types:

- Lead — ['crm', 'CCrmDocumentLead', 'LEAD_777']
- Company — ['crm', 'CCrmDocumentCompany', 'COMPANY_777']
- Contact — ['crm', 'CCrmDocumentContact', 'CONTACT_777']
- Deal — ['crm', 'CCrmDocumentDeal', 'DEAL_777']
- Disk file — ['disk', 'Bitrix\Disk\BizProcDocument', '777']
- Process document in the news feed — ['lists', 'BizprocDocument', '777']
- List document — ['lists', 'Bitrix\Lists\BizprocDocumentLists', '777']
- Smart process element — ['crm', 'Bitrix\Crm\Integration\BizProc\Document\Dynamic', 'DYNAMIC_147_1'], where `147` is the `ID` of the smart process, `1` is the `ID` of the smart process element
- Invoice — ['crm', 'Bitrix\Crm\Integration\BizProc\Document\SmartInvoice', 'SMART_INVOICE_3']
||
|| **PARAMETERS**
[`object`](../data-types.md) | Values of the parameters for the business process template.

Used if the template has parameters.

To pass a value to a parameter of type "User binding", use a record like `user_ID`. For example:

```php
PARAMETERS: {
    'resp_employee': user_14 // Employee ID — 14
}
```
||
|#

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TEMPLATE_ID":1,"DOCUMENT_ID":["crm","CCrmDocumentLead","LEAD_1"],"PARAMETERS":{"Parameter1":"user_1"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.workflow.start
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TEMPLATE_ID":1,"DOCUMENT_ID":["crm","CCrmDocumentLead","LEAD_1"],"PARAMETERS":{"Parameter1":"user_1"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.workflow.start
    ```

- JS

    ```js
    BX24.callMethod(	
        'bizproc.workflow.start',
        {
            TEMPLATE_ID: 1,
            DOCUMENT_ID: [
                'crm',
                'CCrmDocumentLead',
                'LEAD_1'
            ],
            PARAMETERS: {
                'Parameter1': 'user_1'
            },
        },
        function(result) {
            console.log('response', result.answer);
            if(result.error())
                alert("Error: " + result.error());
            else
            console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.workflow.start',
        [
            'TEMPLATE_ID' => 1,
            'DOCUMENT_ID' => [
                'crm',
                'CCrmDocumentLead',
                'LEAD_1'
            ],
            'PARAMETERS' => [
                'Parameter1' => 'user_1'
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
    "result": "66e81a641752f8.56521481",
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
[`string`](../data-types.md) | Root element of the response.

Returns the identifier of the started business process ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_WRONG_WORKFLOW_ID",
    "error_description": "Empty TEMPLATE_ID"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** |**Code** | **Description** | **Value** ||
|| `400` | `ERROR_WRONG_WORKFLOW_ID` | Empty TEMPLATE_ID | An empty input parameter `TEMPLATE_ID` was provided ||
|| `400` | `ERROR_WRONG_WORKFLOW_ID` | Template not found | The business process template was not found for the provided `TEMPLATE_ID` ||
|| `400` | Empty value | Wrong DOCUMENT_ID! | An incorrect `DOCUMENT_ID` was provided ||
|| `400` | Empty value | Incorrect document type! | Unable to determine the document type from the provided `DOCUMENT_ID` ||
|| `400` | Empty value | Template type and DOCUMENT_ID mismatch! | The document type in the template does not match the type determined from `DOCUMENT_ID`.

Attempt to start the template on the wrong entity for which it was created ||
|| `403` | `ACCESS_DENIED` | Access denied! | The method was executed by a non-administrator ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-workflow-instances.md)
- [{#T}](./bizproc-workflow-terminate.md)
- [{#T}](./bizproc-workflow-kill.md)