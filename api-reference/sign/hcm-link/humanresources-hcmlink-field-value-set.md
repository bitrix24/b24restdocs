# Transfer HCM Link Field Values humanresources.hcmlink.field.value.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources.hcmlink`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `humanresources.hcmlink.field.value.set` method transfers HR system field values for employees.

The method can only be executed in the context of authorization of the [application](../../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **company***
[`string`](../../data-types.md) | Company code in the HR system or CRM company ID.

You can get the company code using [humanresources.hcmlink.company.list](./humanresources-hcmlink-company-list.md).

You can get the CRM company ID using [crm.item.list](../../crm/universal/crm-item-list.md) with the `entityTypeId = 4` parameter and the `isMyCompany = Y` filter. If `job` is passed, the company can be determined by the job ||
|| **data***
[`array`](../../data-types.md) | List of field values [(detailed description)](#data) ||
|| **job**
[`object`](../../data-types.md) | Data for updating the synchronization job [(detailed description)](#job).

Pass the parameter if the method is executed in response to the `OnHumanResourcesHcmLinkFieldValueRequested`, `OnHumanResourcesHcmLinkPinRequested`, or `OnHumanResourcesHcmLinkSalaryVacationRequested` event. The job ID is passed in the event in the `jobId` field ||
|#

### data Array Element {#data}

#|
|| **Name**
`type` | **Description** ||
|| **field***
[`string`](../../data-types.md) | Field code in the HR system.

You can get the code from `fields[].field` in the response of [humanresources.hcmlink.company.list](./humanresources-hcmlink-company-list.md) ||
|| **employee***
[`string`](../../data-types.md) | Employee code in the HR system.

You can get the code from `employees[].employee` in the response of [humanresources.hcmlink.employee.list](./humanresources-hcmlink-employee-list.md) ||
|| **value***
[`string`](../../data-types.md) | Field value ||
|#

### job Parameter {#job}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Synchronization task identifier.

Comes in events `OnHumanResourcesHcmLinkFieldValueRequested`, `OnHumanResourcesHcmLinkPinRequested`, or `OnHumanResourcesHcmLinkSalaryVacationRequested` in the field `jobId` ||
|| **fields***
[`object`](../../data-types.md) | New synchronization task data [(detailed description)](#job-fields) ||
|#

### job.fields Parameter {#job-fields}

#|
|| **Name**
`type` | **Description** ||
|| **status***
[`string`](../../data-types.md) | New task status.

Possible values:

- `IN_PROGRESS` — in progress
- `DONE` — completed
- `CANCELED` — cancelled ||
|| **total**
[`integer`](../../data-types.md) | Total number of items in the task ||
|| **sent**
[`integer`](../../data-types.md) | Number of processed items ||
|| **data**
[`object`](../../data-types.md) | Additional task data ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"company":"hr-company-001","data":[{"field":"personal_number","employee":"employee-001","value":"TN-1001"}],"job":{"id":101,"fields":{"status":"DONE","total":1,"sent":1}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/humanresources.hcmlink.field.value.set
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make({
        method: 'humanresources.hcmlink.field.value.set',
        params: {
          company: 'hr-company-001',
          data: [
            {
              field: 'personal_number',
              employee: 'employee-001',
              value: 'TN-1001',
            },
          ],
          job: {
            id: 101,
            fields: {
              status: 'DONE',
              total: 1,
              sent: 1,
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info(response.getData()!.result)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function setHcmLinkFieldValues() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'humanresources.hcmlink.field.value.set',
            params: {
              company: 'hr-company-001',
              data: [
                {
                  field: 'personal_number',
                  employee: 'employee-001',
                  value: 'TN-1001'
                }
              ],
              job: {
                id: 101,
                fields: {
                  status: 'DONE',
                  total: 1,
                  sent: 1
                }
              }
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info(response.getData().result)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setHcmLinkFieldValues)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.hcmlink.field.value.set',
                [
                    'company' => 'hr-company-001',
                    'data' => [
                        [
                            'field' => 'personal_number',
                            'employee' => 'employee-001',
                            'value' => 'TN-1001',
                        ],
                    ],
                    'job' => [
                        'id' => 101,
                        'fields' => [
                            'status' => 'DONE',
                            'total' => 1,
                            'sent' => 1,
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting field values: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'humanresources.hcmlink.field.value.set',
        {
            company: 'hr-company-001',
            data: [{ field: 'personal_number', employee: 'employee-001', value: 'TN-1001' }],
            job: { id: 101, fields: { status: 'DONE', total: 1, sent: 1 } }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.hcmlink.field.value.set',
        [
            'company' => 'hr-company-001',
            'data' => [
                [
                    'field' => 'personal_number',
                    'employee' => 'employee-001',
                    'value' => 'TN-1001',
                ],
            ],
            'job' => [
                'id' => 101,
                'fields' => [
                    'status' => 'DONE',
                    'total' => 1,
                    'sent' => 1,
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see section "SDK for Go"
    res, err := client.Core().Call(ctx, "humanresources.hcmlink.field.value.set", b24.Params{
    	"company": "hr-company-001",
    	"data": []b24.Params{
    		{
    			"field":    "personal_number",
    			"employee": "employee-001",
    			"value":    "TN-1001",
    		},
    	},
    	"job": b24.Params{
    		"id": 101,
    		"fields": b24.Params{
    			"status": "DONE",
    			"total":  1,
    			"sent":   1,
    		},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("humanresources.hcmlink.field.value.set: %w", err)
    }

    var result struct {
    	Status string   `json:"status"`
    	Errors []string `json:"errors"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("response breakdown: %w", err)
    }
    fmt.Println(result.Status, result.Errors)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "status": "ok",
        "errors": []
    },
    "time": {
        "start": 1739860000.123,
        "finish": 1739860000.456,
        "duration": 0.333,
        "processing": 0.111,
        "date_start": "2026-08-06T19:51:02+03:00",
        "date_finish": "2026-08-06T19:51:02+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Field value processing result ||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

#### result Object Fields

#|
|| **Name**
`type` | **Description** ||
|| **status**
[`string`](../../data-types.md) | Processing status.

Possible values:

- `ok` — values were processed without errors
- `error` — errors occurred during processing ||
|| **errors**
[`array`](../../data-types.md) | List of item processing errors ||
|#

## Error Handling

HTTP status: **200**, **403**

```json
{
    "result": {
        "status": "error",
        "errors": [
            "Parameter 'data' must be a non empty array "
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **When It Occurs** ||
|| `0` | Operation failed | Failed to update the synchronization job passed in the `job` parameter ||
|| `-` | Parameter 'data' must be a non empty array | The field value list is not passed ||
|| `-` | Item #... field '...' not found for company '...' | The field was not found in the company ||
|| `-` | Item #... employee '...' not found for company '...' | The employee was not found in the company ||
|| `-` | Item #... does not match PIN request job '...' | The value does not match the PIN request job ||
|| `-` | Item #... does not match salary/vacation request job '...' | The value does not match the payroll or vacation balance request job ||
|| `-` | Job '...' result is incomplete | The `DONE` status was passed for the job, but the response does not contain all requested values ||
|| `ACCESS_DENIED` | Access denied! Access denied. | The user is not an administrator ||
|| `WRONG_AUTH_TYPE` | Application context required | The method was not called in the application context ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-hcmlink-employee-list.md)
- [{#T}](./humanresources-hcmlink-job-update.md)
- [{#T}](./humanresources-hcmlink-job-status-get.md)
