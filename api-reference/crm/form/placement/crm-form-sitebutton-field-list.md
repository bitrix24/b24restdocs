# Get a List of Website Widget Fields crm.form.sitebutton.field.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.form.sitebutton.field.list` method returns a list of available website widget fields for a CRM form.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | A list of description fields to return in the response. If the parameter is not passed, the method returns all description fields.

Available fields:

- `name` — field name
- `type` — data type
- `title` — header
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — mandatory groups
- `filterable` — filter availability flag
- `sortable` — sorting availability flag
- `editable` — editability flag
- `editableGroups` — methods in which the field can be changed
- `multiple` — multiple value flag
- `elementType` — item type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.sitebutton.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","filterable","sortable","editable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.sitebutton.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","filterable","sortable","editable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.sitebutton.field.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type Result = {
      items: Array<{
        name?: string
        type?: string
        filterable?: boolean
        sortable?: boolean
        editable?: boolean
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<Result>({
        method: 'crm.form.sitebutton.field.list',
        params: {
          select: [
            'name',
            'type',
            'filterable',
            'sortable',
            'editable',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.items)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function crmFormSitebuttonFieldList() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.sitebutton.field.list',
            params: {
              select: [
                'name',
                'type',
                'filterable',
                'sortable',
                'editable',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', crmFormSitebuttonFieldList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    select = [
        "name",
        "type",
        "filterable",
        "sortable",
        "editable",
    ]

    try:
        bitrix_response = client.crm.form.sitebutton.field.list(
            select=select,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.form.sitebutton.field.list',
                [
                    'select' => [
                        'name',
                        'type',
                        'filterable',
                        'sortable',
                        'editable',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.form.sitebutton.field.list',
        {
            select: [
                'name',
                'type',
                'filterable',
                'sortable',
                'editable',
            ],
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.form.sitebutton.field.list',
        [
            'select' => [
                'name',
                'type',
                'filterable',
                'sortable',
                'editable',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    res, err := client.Core().Call(ctx, "crm.form.sitebutton.field.list", b24.Params{
    	"select": []string{"name", "type", "filterable", "sortable", "editable"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.form.sitebutton.field.list: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("no items key in the response")
    }

    var items []struct {
    	Name       string `json:"name"`
    	Type       string `json:"type"`
    	Filterable bool   `json:"filterable"`
    	Sortable   bool   `json:"sortable"`
    	Editable   bool   `json:"editable"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range items {
    	fmt.Println(item.Name, item.Type)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "name": "formId",
                "type": "int",
                "filterable": true,
                "sortable": false,
                "editable": false
            },
            {
                "name": "id",
                "type": "int",
                "filterable": false,
                "sortable": true,
                "editable": false
            },
            {
                "name": "name",
                "type": "string",
                "filterable": false,
                "sortable": false,
                "editable": false
            },
            {
                "name": "system",
                "type": "bool",
                "filterable": false,
                "sortable": false,
                "editable": false
            },
            {
                "name": "bound",
                "type": "bool",
                "filterable": false,
                "sortable": false,
                "editable": false
            },
            {
                "name": "formsId",
                "type": "array",
                "filterable": false,
                "sortable": false,
                "editable": false
            },
            {
                "name": "previewUrl",
                "type": "string",
                "filterable": false,
                "sortable": false,
                "editable": false
            }
        ]
    },
    "time": {
        "start": 1792594764,
        "finish": 1792594764.448954,
        "duration": 0.44895410537719727,
        "processing": 0,
        "date_start": "2026-10-21T17:59:24+02:00",
        "date_finish": "2026-10-21T17:59:24+02:00",
        "operating_reset_at": 1792595340,
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
[`array`](../../../data-types.md) | Array of field descriptions. The content of each description depends on `select` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field \"foo\" found in entity \"DtoFieldDto\"."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check user permissions and scope `crm` ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `foo` found in entity `DtoFieldDto` | Pass `select` as an array of strings from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `editableGroups`, `multiple`, `elementType` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-sitebutton-field-get.md)
- [{#T}](./crm-form-sitebutton-list.md)
- [{#T}](./index.md)
