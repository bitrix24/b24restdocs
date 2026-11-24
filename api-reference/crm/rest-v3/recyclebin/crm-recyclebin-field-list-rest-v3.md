# Get a List of CRM Recycle Bin Record Fields v 3.0 crm.recyclebin.field.list

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

The `crm.recyclebin.field.list` method returns a list of available CRM Recycle Bin record fields.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of description fields to return, for example, `["name", "type", "filterable"]`. [Description fields](#item).

By default, the method returns all description fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.recyclebin.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","description","filterable","sortable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.recyclebin.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","description","filterable","sortable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.recyclebin.field.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldListResult = {
      items: Array<{
        name: string
        type: string
        description: string | null
        filterable: boolean
        sortable: boolean
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<FieldListResult>(
        'crm.recyclebin.field.list',
        {
          select: ['name', 'type', 'description', 'filterable', 'sortable'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Fields:', result.items)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.recyclebin.field.list',
            {
              select: ['name', 'type', 'description', 'filterable', 'sortable'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Fields:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listFields)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.recyclebin.field.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["name", "type", "description", "filterable", "sortable"],
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["items"])
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.recyclebin.field.list',
                [
                    'select' => ['name', 'type', 'description', 'filterable', 'sortable'],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.recyclebin.field.list',
        {
            select: ['name', 'type', 'description', 'filterable', 'sortable'],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.recyclebin.field.list',
        [
            'select' => ['name', 'type', 'description', 'filterable', 'sortable'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.recyclebin.field.list", b24.Params{
    	"select": []string{"name", "type", "description", "filterable", "sortable"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.recyclebin.field.list: %w", err)
    }

    var out struct {
    	Items []struct {
    		Name        string  `json:"name"`
    		Type        string  `json:"type"`
    		Description *string `json:"description"`
    		Filterable  bool    `json:"filterable"`
    		Sortable    bool    `json:"sortable"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, field := range out.Items {
    	fmt.Println(field.Name, field.Type, field.Filterable, field.Sortable)
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
                "name": "recordId",
                "type": "int",
                "description": "Recycle bin record ID used by crm.recyclebin.restore.",
                "filterable": false,
                "sortable": true
            },
            {
                "name": "entityTypeId",
                "type": "int",
                "description": "CRM type: deal, lead, contact, company or an existing restorable smart process.",
                "filterable": true,
                "sortable": false
            },
            {
                "name": "id",
                "type": "int",
                "description": "Former CRM item ID before deletion, distinct from the recycle bin record ID.",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "title",
                "type": "string",
                "description": "Item title at deletion. Filtering supports exact equality.",
                "filterable": true,
                "sortable": true
            },
            {
                "name": "deletedTime",
                "type": "object",
                "description": "Deletion date and time in ISO 8601 format.",
                "filterable": true,
                "sortable": true
            },
            {
                "name": "deletedById",
                "type": "int",
                "description": "ID of the user who deleted the item.",
                "filterable": true,
                "sortable": true
            }
        ]
    },
    "time": {
        "start": 1795431028,
        "finish": 1795431028.276237,
        "duration": 0.27623701095581055,
        "processing": 0,
        "date_start": "2026-11-23T11:50:28+01:00",
        "date_finish": "2026-11-23T11:50:28+01:00",
        "operating_reset_at": 1795431600,
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
|| **result.items**
[`object[]`](../../../data-types.md) | Array of Recycle Bin record field descriptions. [Description fields](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

The method returns six fields: `recordId`, `entityTypeId`, `id`, `title`, `deletedTime`, and `deletedById`. Their values are described in the [Recycle Bin record fields table](./crm-recyclebin-list-rest-v3.md#item). The description of `deletedTime` specifies the `object` type, but the [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md) response contains a date and time string, for example, `2026-11-23T11:52:45+01:00`.

#### Description Fields {#item}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Field name, for example, `recordId` ||
|| **type**
[`string`](../../../data-types.md) | Data type: `int`, `string`, or `object` ||
|| **title**
[`string`](../../../data-types.md) | Field title. It matches `name` ||
|| **description**
[`string`](../../../data-types.md) | Field description in English ||
|| **validationRules**
[`array`](../../../data-types.md) | Value validation rules. For Recycle Bin record fields — an empty array ||
|| **requiredGroups**
[`array`](../../../data-types.md) | Methods that require this field. For Recycle Bin record fields — `null` ||
|| **filterable**
[`boolean`](../../../data-types.md) | Whether the field can be used for filtering in the [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md) method ||
|| **sortable**
[`boolean`](../../../data-types.md) | Whether the field can be used for sorting in the [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md) method ||
|| **editable**
[`boolean`](../../../data-types.md) | Whether the field can be changed. For Recycle Bin record fields — `false` ||
|| **editableGroups**
[`array`](../../../data-types.md) | Methods in which the field can be changed. For Recycle Bin record fields — `null` ||
|| **multiple**
[`boolean`](../../../data-types.md) | Whether the field is multiple. For Recycle Bin record fields — `false` ||
|| **elementType**
[`string`](../../../data-types.md) | Element type for composite fields. For Recycle Bin record fields — `null` ||
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

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `DtoFieldDto` | Check the names in `select` against the [description fields table](#item) ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | REST 3.0 CRM Recycle Bin methods are not available on this Bitrix24 ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-recyclebin-field-get-rest-v3.md)
- [{#T}](./crm-recyclebin-list-rest-v3.md)
