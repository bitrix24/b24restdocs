# Update HCM Link Job humanresources.hcmlink.job.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources.hcmlink`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `humanresources.hcmlink.job.update` method updates an HCM Link synchronization job.

The method can only be executed in the context of authorization of the [application](../../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Synchronization job ID.

The ID is passed in the `jobId` field of the `OnHumanResourcesHcmLinkEmployeeListRequested`, `OnHumanResourcesHcmLinkFieldValueRequested`, `OnHumanResourcesHcmLinkEmployeeListMapped`, `OnHumanResourcesHcmLinkPinRequested`, or `OnHumanResourcesHcmLinkSalaryVacationRequested` events ||
|| **fields***
[`object`](../../data-types.md) | Job data [(detailed description)](#fields) ||
|#

### fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **status***
[`string`](../../data-types.md) | New job status.

Possible values:

- `IN_PROGRESS` — in progress
- `DONE` — completed
- `CANCELED` — canceled ||
|| **total**
[`integer`](../../data-types.md) | Total number of items in the job ||
|| **sent**
[`integer`](../../data-types.md) | Number of processed items ||
|| **data**
[`object`](../../data-types.md) | Additional job data ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":101,"fields":{"status":"DONE","total":10,"sent":10,"data":{"batch":"2026-08-06"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/humanresources.hcmlink.job.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make({
        method: 'humanresources.hcmlink.job.update',
        params: {
          id: 101,
          fields: {
            status: 'DONE',
            total: 10,
            sent: 10,
            data: {
              batch: '2026-08-06',
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
      async function updateHcmLinkJob() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'humanresources.hcmlink.job.update',
            params: {
              id: 101,
              fields: {
                status: 'DONE',
                total: 10,
                sent: 10,
                data: {
                  batch: '2026-08-06'
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

      document.addEventListener('DOMContentLoaded', updateHcmLinkJob)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.hcmlink.job.update',
                [
                    'id' => 101,
                    'fields' => [
                        'status' => 'DONE',
                        'total' => 10,
                        'sent' => 10,
                        'data' => [
                            'batch' => '2026-08-06',
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
        echo 'Error updating job: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'humanresources.hcmlink.job.update',
        { id: 101, fields: { status: 'DONE', total: 10, sent: 10, data: { batch: '2026-08-06' } } },
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
        'humanresources.hcmlink.job.update',
        [
            'id' => 101,
            'fields' => [
                'status' => 'DONE',
                'total' => 10,
                'sent' => 10,
                'data' => [
                    'batch' => '2026-08-06',
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
    // client and ctx are already created — see "SDK for Go" section
    res, err := client.Core().Call(ctx, "humanresources.hcmlink.job.update", b24.Params{
    	"id": 101,
    	"fields": b24.Params{
    		"status": "DONE",
    		"total":  10,
    		"sent":   10,
    		"data": b24.Params{
    			"batch": "2026-08-06",
    		},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("humanresources.hcmlink.job.update: %w", err)
    }

    var updated bool
    if err := json.Unmarshal(res.Result, &updated); err != nil {
    	return fmt.Errorf("response breakdown: %w", err)
    }
    fmt.Println(updated)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
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
[`boolean`](../../data-types.md) | Returns `true` if the job was updated ||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **200**, **403**

```json
{
    "result": {
        "error": 0,
        "error_description": "Operation failed"
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **When It Occurs** ||
|| `0` | Operation failed | The job was not found or an outdated status was passed ||
|| `ACCESS_DENIED` | Access denied! Access denied. | The user is not an administrator ||
|| `WRONG_AUTH_TYPE` | Application context required | The method was not called in the application context ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-hcmlink-job-status-get.md)
- [{#T}](./humanresources-hcmlink-field-value-set.md)
