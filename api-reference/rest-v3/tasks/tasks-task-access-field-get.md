# Get Task Access Field Permissions tasks.task.access.field.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.access.field.get` returns the description of the task access field by name.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | The name of the field whose description is to be retrieved ||
|| **select**
[`array`](../../data-types.md) | A list of description fields to return in the response.

Available fields:
- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — mandatory groups
- `filterable` — filter availability indicator
- `sortable` — sort availability indicator
- `editable` — editability indicator
- `multiple` — multiple value indicator
- `elementType` — element type for composite fields ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.access.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"id","select":["name","type","title","description","filterable","sortable","multiple"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.access.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"id","select":["name","type","title","description","filterable","sortable","multiple"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.access.field.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AccessFieldGetResult = {
      item: {
        name: string
        type: string
        title: string
        description: string | null
        validationRules: unknown[]
        requiredGroups: string[] | null
        filterable: boolean
        sortable: boolean
        editable: boolean
        multiple: boolean
        elementType: string | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<AccessFieldGetResult>({
        method: 'tasks.task.access.field.get',
        params: {
          name: 'id',
          select: [
            'name',
            'type',
            'title',
            'description',
            'filterable',
            'sortable',
            'multiple',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field item:', result.item)
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
      async function getAccessField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'tasks.task.access.field.get',
            params: {
              name: 'id',
              select: [
                'name',
                'type',
                'title',
                'description',
                'filterable',
                'sortable',
                'multiple',
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
          console.info('Field item:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAccessField)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.access.field.get',
                [
                    'name' => 'id',
                    'select' => [
                        'name',
                        'type',
                        'title',
                        'description',
                        'filterable',
                        'sortable',
                        'multiple'
                    ]
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
        'tasks.task.access.field.get',
        {
            name: 'id',
            select: [
                'name',
                'type',
                'title',
                'description',
                'filterable',
                'sortable',
                'multiple'
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
        'tasks.task.access.field.get',
        [
            'name' => 'id',
            'select' => [
                'name',
                'type',
                'title',
                'description',
                'filterable',
                'sortable',
                'multiple'
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
    "result": {
        "item": {
            "name": "id",
            "type": "int",
            "title": "ID",
            "description": "Identifier",
            "validationRules": [],
            "requiredGroups": null,
            "filterable": true,
            "sortable": true,
            "editable": false,
            "multiple": false,
            "elementType": null
        }
    },
    "time": {
        "start": 1769780771,
        "finish": 1769780771.081992,
        "duration": 0.08199191093444824,
        "processing": 0,
        "date_start": "2026-01-30T16:46:11+01:00",
        "date_finish": "2026-01-30T16:46:11+01:00",
        "operating_reset_at": 1769781371,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing the response data ||
|| **item**
[`object`](../../data-types.md) | Object with field description. The response structure depends on `select`  ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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
|| `-` | Access denied | Check user permissions and scope `task` ||
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

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for entity `DtoFieldDto` | Pass only fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings, e.g., `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-access-field-list.md)
- [{#T}](./tasks-task-access-get.md)
- [{#T}](./index.md)
