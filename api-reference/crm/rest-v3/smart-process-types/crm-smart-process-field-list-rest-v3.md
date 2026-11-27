# Get SPA Type Field Descriptions v 3.0 crm.smartprocess.field.list

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

The `crm.smartprocess.field.list` method returns descriptions of SPA type fields: the data type and flags showing whether a field can be passed when writing, filtered by, and sorted by.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of description fields to return, for example, `["name", "type", "editable"]`. [Description fields](#item).

By default, the method returns all description fields ||
|#

The method describes the fields of the SPA type itself, which are returned by the [crm.smartprocess.get](./crm-smart-process-get-rest-v3.md) and [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) methods. SPA item fields are returned by a different method — [crm.smartprocess&lt;entityTypeId&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md).

The method returns the full list of fields: it ignores the `filter`, `order`, and `pagination` parameters without returning an error. `select` can be passed as a string with one field: `"select": "name"`.

The method name is written in lowercase: `crm.smartprocess.field.list`. For the name `crm.smartProcess.field.list`, the method returns the ["Method Not Found"](#method-not-found) error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess.field.list`

{% endnote %}

The examples retrieve the name, data type, and editable flag of each SPA type field.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","editable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","editable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess.field.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldListResult = {
      items: {
        name: string
        type: string
        editable: boolean
      }[]
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<FieldListResult>(
        'crm.smartprocess.field.list',
        {
          select: ['name', 'type', 'editable'],
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
      async function listSmartProcessTypeFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess.field.list',
            {
              select: ['name', 'type', 'editable'],
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

      document.addEventListener('DOMContentLoaded', listSmartProcessTypeFields)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess.field.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={"select": ["name", "type", "editable"]},
            prefer_version=3,
        )
        for field in response["result"]["items"]:
            print(field["name"], field["type"], field["editable"])
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
                'crm.smartprocess.field.list',
                [
                    'select' => ['name', 'type', 'editable'],
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
        'crm.smartprocess.field.list',
        {
            select: ['name', 'type', 'editable'],
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
        'crm.smartprocess.field.list',
        [
            'select' => ['name', 'type', 'editable'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess.field.list", b24.Params{
    	"select": []string{"name", "type", "editable"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess.field.list: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("response has no items key")
    }

    var fields []struct {
    	Name     string `json:"name"`
    	Type     string `json:"type"`
    	Editable bool   `json:"editable"`
    }
    if err := json.Unmarshal(raw, &fields); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, f := range fields {
    	fmt.Println(f.Name, f.Type, f.Editable)
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
                "name": "entityTypeId",
                "type": "int",
                "title": "entityTypeId",
                "description": null,
                "validationRules": [],
                "requiredGroups": null,
                "filterable": true,
                "sortable": true,
                "editable": false,
                "editableGroups": null,
                "multiple": false,
                "elementType": null
            },
            {
                "name": "title",
                "type": "string",
                "title": "title",
                "description": null,
                "validationRules": [],
                "requiredGroups": [
                    "add"
                ],
                "filterable": true,
                "sortable": true,
                "editable": true,
                "editableGroups": [],
                "multiple": false,
                "elementType": null
            }
        ]
    },
    "time": {
        "start": 1795430991,
        "finish": 1795430991.241357,
        "duration": 0.24135708808898926,
        "processing": 0,
        "date_start": "2026-11-23T11:49:51+01:00",
        "date_finish": "2026-11-23T11:49:51+01:00",
        "operating_reset_at": 1795431540,
        "operating": 0
    }
}
```

The example shows the first two of 36 fields. The response was received without `select`.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.items**
[`array`](../../../data-types.md) | Array of field descriptions. [Description fields](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Description Fields {#item}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Field name in camelCase, for example, `title` ||
|| **type**
[`string`](../../../data-types.md) | Data type: `int`, `string`, `bool`, `object`, or `array`. For the `createdTime` and `updatedTime` dates, it is `object`, although the date is returned as a string in ISO 8601 format ||
|| **elementType**
[`string`](../../../data-types.md) | Element type of the `customSectionIds` field. For other fields, `null` ||
|| **multiple**
[`boolean`](../../../data-types.md) | Whether the field is multiple. For all SPA type fields, `false` ||
|| **title**
[`string`](../../../data-types.md) | Field title. Currently matches `name` ||
|| **description**
[`string`](../../../data-types.md) | Field description. If there is no description, `null` ||
|| **validationRules**
[`array`](../../../data-types.md) | Value validation rules. For SPA type fields, an empty array; for `description`, `[{}]` ||
|| **requiredGroups**
[`array`](../../../data-types.md) | Methods that require this field: `["add"]` for `title`. For other fields, `null` ||
|| **filterable**
[`boolean`](../../../data-types.md) | Whether the field can be used in a filter of the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md#filter) method ||
|| **sortable**
[`boolean`](../../../data-types.md) | Whether the field can be used for sorting in the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md#order) method ||
|| **editable**
[`boolean`](../../../data-types.md) | Whether the field can be passed in `fields` of the [crm.smartprocess.add](./crm-smart-process-add-rest-v3.md) and [crm.smartprocess.update](./crm-smart-process-update-rest-v3.md) methods ||
|| **editableGroups**
[`array`](../../../data-types.md) | Methods in which the field can be passed. An empty array `[]` for an editable field means there are no restrictions. For read-only fields, `null` ||
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

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.smartProcess.field.list` was not found | Write the method name in lowercase: `crm.smartprocess.field.list` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-smart-process-field-get-rest-v3.md)
- [{#T}](./crm-smart-process-list-rest-v3.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
