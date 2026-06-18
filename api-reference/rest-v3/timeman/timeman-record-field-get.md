# Get the Record Field of Working Time timeman.record.field.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.record.field.get` returns the description of the working time record field by name.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **name*** 
[`string`](../../data-types.md) | The name of the field whose description is to be retrieved.

Available fields:

- `id` — record identifier
- `userId` — employee identifier
- `startTime` — start time of the working interval
- `endTime` — end time of the working interval
- `duration` — duration of the working interval in seconds
- `breakLength` — duration of breaks in seconds
- `state` — status of the record
- `isApproved` — approval status of the record ||
|| **select**
[`array`](../../data-types.md) | List of description fields to return in the response.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required groups
- `filterable` — filter availability status
- `sortable` — sort availability status
- `editable` — editability status
- `multiple` — multiple value status
- `elementType` — element type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/timeman.record.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"startTime","select":["name","type","title","filterable","sortable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/timeman.record.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"startTime","select":["name","type","title","filterable","sortable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/timeman.record.field.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FieldGetResult = {
      item: {
        name: string
        type: string
        title: string
        filterable: boolean
        sortable: boolean
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<FieldGetResult>({
        method: 'timeman.record.field.get',
        params: {
          name: 'startTime',
          select: [
            'name',
            'type',
            'title',
            'filterable',
            'sortable',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.name, result.item.type, result.item.title)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function fetchRecordField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'timeman.record.field.get',
            params: {
              name: 'startTime',
              select: [
                'name',
                'type',
                'title',
                'filterable',
                'sortable',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.name, result.item.type, result.item.title)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchRecordField)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.record.field.get',
                [
                    'name' => 'startTime',
                    'select' => [
                        'name',
                        'type',
                        'title',
                        'filterable',
                        'sortable'
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'timeman.record.field.get',
        {
            name: 'startTime',
            select: [
                'name',
                'type',
                'title',
                'filterable',
                'sortable'
            ]
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.record.field.get',
        [
            'name' => 'startTime',
            'select' => [
                'name',
                'type',
                'title',
                'filterable',
                'sortable'
            ],
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
    "result": {
        "item": {
            "name": "startTime",
            "type": "object",
            "title": "startTime",
            "filterable": true,
            "sortable": true
        }
    },
    "time": {
        "start": 1780410633,
        "finish": 1780410633.157809,
        "duration": 0.15780901908874512,
        "processing": 0,
        "date_start": "2026-06-02T17:30:33+02:00",
        "date_finish": "2026-06-02T17:30:33+02:00",
        "operating_reset_at": 1780411233,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data ||
|| **item**
[`object`](../../data-types.md) | Object with field description. The response structure depends on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "field": "name",
                "message": "Required field `name` is not specified"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Check read permissions for working time and scope `timeman` ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `name` | Field `#FIELD#` not found | Specify an existing field name ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `name` | Required field `name` is not specified | Pass the `name` parameter with an existing field name ||
|#

#### Select Parameter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for object `DtoFieldDto` | Pass only fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings, e.g., `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./timeman-record-field-list.md)
- [{#T}](./timeman-record-list.md)
- [{#T}](./index.md)
