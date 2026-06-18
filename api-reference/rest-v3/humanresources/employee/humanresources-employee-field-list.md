# Get the list of employee fields humanresources.employee.field.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `humanresources.employee.field.list` returns a list of available employee fields.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of field descriptions to return in the response.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required groups
- `filterable` — filter availability indicator
- `sortable` — sort availability indicator
- `editable` — editability indicator
- `multiple` — multiple value indicator
- `elementType` — element type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.employee.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","description","validationRules","requiredGroups","filterable","sortable","editable","multiple","elementType"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.employee.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","description","validationRules","requiredGroups","filterable","sortable","editable","multiple","elementType"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.employee.field.list
    ```


- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldItem = {
      name: string
      type: string
      title: string
      description: string | null
      validationRules: unknown[]
      requiredGroups: unknown | null
      filterable: boolean
      sortable: boolean
      editable: boolean
      multiple: boolean
      elementType: string | null
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EmployeeFieldListResult = {
      items: FieldItem[]
    }

    try {
      const response = await $b24.actions.v3.call.make<EmployeeFieldListResult>({
        method: 'humanresources.employee.field.list',
        params: {
          select: [
            'name',
            'type',
            'title',
            'description',
            'validationRules',
            'requiredGroups',
            'filterable',
            'sortable',
            'editable',
            'multiple',
            'elementType',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Employee fields count:', result.items.length, result.items)
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
      async function getEmployeeFieldList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.employee.field.list',
            params: {
              select: [
                'name',
                'type',
                'title',
                'description',
                'validationRules',
                'requiredGroups',
                'filterable',
                'sortable',
                'editable',
                'multiple',
                'elementType',
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
          console.info('Employee fields count:', result.items.length, result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEmployeeFieldList)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.employee.field.list',
                [
                    'select' => [
                        'name',
                        'type',
                        'title',
                        'description',
                        'validationRules',
                        'requiredGroups',
                        'filterable',
                        'sortable',
                        'editable',
                        'multiple',
                        'elementType',
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
        'humanresources.employee.field.list',
        {
            select: [
                'name',
                'type',
                'title',
                'description',
                'validationRules',
                'requiredGroups',
                'filterable',
                'sortable',
                'editable',
                'multiple',
                'elementType'
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
        'humanresources.employee.field.list',
        [
            'select' => [
                'name',
                'type',
                'title',
                'description',
                'validationRules',
                'requiredGroups',
                'filterable',
                'sortable',
                'editable',
                'multiple',
                'elementType',
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
        "items": [
            {
                "description": null,
                "editable": false,
                "elementType": null,
                "filterable": false,
                "multiple": false,
                "name": "userId",
                "requiredGroups": null,
                "sortable": false,
                "title": "userId",
                "type": "int",
                "validationRules": []
            },
            {
                "description": null,
                "editable": false,
                "elementType": null,
                "filterable": false,
                "multiple": false,
                "name": "name",
                "requiredGroups": null,
                "sortable": false,
                "title": "name",
                "type": "string",
                "validationRules": []
            },
            {
                "description": null,
                "editable": false,
                "elementType": null,
                "filterable": false,
                "multiple": false,
                "name": "workPosition",
                "requiredGroups": null,
                "sortable": false,
                "title": "workPosition",
                "type": "string",
                "validationRules": []
            },
            {
                "description": null,
                "editable": false,
                "elementType": null,
                "filterable": false,
                "multiple": false,
                "name": "avatar",
                "requiredGroups": null,
                "sortable": false,
                "title": "avatar",
                "type": "string",
                "validationRules": []
            },
            {
                "description": null,
                "editable": false,
                "elementType": null,
                "filterable": false,
                "multiple": false,
                "name": "url",
                "requiredGroups": null,
                "sortable": false,
                "title": "url",
                "type": "string",
                "validationRules": []
            },
            {
                "description": null,
                "editable": false,
                "elementType": null,
                "filterable": false,
                "multiple": false,
                "name": "departments",
                "requiredGroups": null,
                "sortable": false,
                "title": "departments",
                "type": "array",
                "validationRules": []
            },
            {
                "description": null,
                "editable": false,
                "elementType": null,
                "filterable": false,
                "multiple": false,
                "name": "teams",
                "requiredGroups": null,
                "sortable": false,
                "title": "teams",
                "type": "array",
                "validationRules": []
            }
        ]
    },
    "time": {
        "start": 1780407800,
        "finish": 1780407800.071511,
        "duration": 0.07151103019714355,
        "processing": 0,
        "date_start": "2026-06-02T16:43:20+02:00",
        "date_finish": "2026-06-02T16:43:20+02:00",
        "operating_reset_at": 1780408400,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **items**
[`array`](../../../data-types.md) | Array of objects with field descriptions. The response structure depends on `select` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Errors in the `select` parameter

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for object `DtoFieldDto` | Only pass fields from the description list ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-employee-field-get.md)
- [{#T}](./index.md)
