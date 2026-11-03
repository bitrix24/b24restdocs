# Get Field Descriptions v 3.0 crm.[type].field.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read items of this CRM object type

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.field.list` method returns a list of available fields of a CRM object type with their attributes: whether a field can be edited, used for filtering, and used for sorting. The object type is set by the `<type>` segment of the method name.

The set of fields depends on the object type and, for an SPA, also on the type settings. The fields that appear with each setting are shown on the [SPA fields](../items/fields-smart-process.md) page. The response includes standard fields, related objects, for example, `assignedBy`, and custom fields together with their [paired `UF_CRM_…_data` fields](../items/fields-smart-process.md#userfields). Field descriptions with sample values are given on the field pages of each object type, linked in the table below.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.field.list` | [Deal fields](../items/fields-deal.md) ||
|| Lead | `crm.lead.field.list` | [Lead fields](../items/fields-lead.md) ||
|| Contact | `crm.contact.field.list` | [Contact fields](../items/fields-contact.md) ||
|| Company | `crm.company.field.list` | [Company fields](../items/fields-company.md) ||
|| Estimate | `crm.quote.field.list` | [Estimate fields](../items/fields-quote.md) ||
|| Invoice | `crm.smartinvoice.field.list` | [Invoice fields](../items/fields-smart-invoice.md) ||
|| My company | `crm.mycompany.field.list` | [My company fields](../items/fields-my-company.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.field.list`, for example, `crm.smartprocess1038.field.list` | [SPA fields](../items/fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. SPAs have field description methods regardless of the type settings.

The method name is written in lowercase: `crm.smartinvoice.field.list`, not `crm.smartInvoice.field.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of description fields to return, for example, `["name", "type", "editable"]`. [Description fields](#item).

By default, the method returns all description fields ||
|#

The method ignores the `filter`, `order`, and `pagination` parameters: if they are passed, it returns the full list of fields without an error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.field.list`

{% endnote %}

The examples retrieve the names, titles, types, and attributes of the fields of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","multiple","editable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","multiple","editable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.field.list
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
        title: string
        multiple: boolean
        editable: boolean
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<FieldListResult>(
        'crm.smartprocess1038.field.list',
        {
          select: ['name', 'type', 'title', 'multiple', 'editable'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach((field) => console.info(field.name, field.type, field.editable))
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
            'crm.smartprocess1038.field.list',
            {
              select: ['name', 'type', 'title', 'multiple', 'editable'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach((field) => console.info(field.name, field.type, field.editable))
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

    method = "crm.smartprocess1038.field.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["name", "type", "title", "multiple", "editable"],
            },
            prefer_version=3,
        )
        result = response["result"]
        for field in result["items"]:
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.field.list',
                [
                    'select' => ['name', 'type', 'title', 'multiple', 'editable'],
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.field.list',
        {
            select: ['name', 'type', 'title', 'multiple', 'editable'],
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.field.list',
        [
            'select' => ['name', 'type', 'title', 'multiple', 'editable'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.field.list", b24.Params{
    	"select": []string{"name", "type", "title", "multiple", "editable"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.field.list: %w", err)
    }

    var out struct {
    	Items []struct {
    		Name     string `json:"name"`
    		Type     string `json:"type"`
    		Editable bool   `json:"editable"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, f := range out.Items {
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
                "name": "title",
                "type": "string",
                "multiple": false,
                "title": "Name",
                "editable": true
            },
            {
                "name": "xmlId",
                "type": "string",
                "multiple": false,
                "title": "External ID",
                "editable": true
            },
            {
                "name": "id",
                "type": "int",
                "multiple": false,
                "title": "id",
                "editable": false
            }
        ]
    },
    "time": {
        "start": 1793714999,
        "finish": 1793714999.191011,
        "duration": 0.1910109519958496,
        "processing": 0,
        "date_start": "2026-11-03T17:09:59+03:00",
        "date_finish": "2026-11-03T17:09:59+03:00",
        "operating_reset_at": 1793715540,
        "operating": 0
    }
}
```

The example shows the first three of 61 fields.

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
[`string`](../../../data-types.md) | Field name: in camelCase for a standard field, for example, `title`, and in the `UF_CRM_…` format for a custom field ||
|| **type**
[`string`](../../../data-types.md) | Data type, for example, `string`, `int`, `date-time`, `object` ||
|| **elementType**
[`string`](../../../data-types.md) | Element type for composite fields, for example, `object` for a related object. For simple fields, `null` ||
|| **multiple**
[`boolean`](../../../data-types.md) | Whether the field accepts multiple values ||
|| **title**
[`string`](../../../data-types.md) | Field title in the Bitrix24 language ||
|| **description**
[`string`](../../../data-types.md) | Field description. If there is no description, `null` ||
|| **validationRules**
[`array`](../../../data-types.md) | Value validation rules, for example, `{"name": "Length", "parameters": {"max": 255}}` — a length of no more than 255 characters ||
|| **requiredGroups**
[`array`](../../../data-types.md) | Methods in which the field is required at the method level, for example, `["add"]`. If there are no such methods, `null`. The requirement set with [crm.&lt;type&gt;.field.required.*](./crm-entity-field-required-get-rest-v3.md) is not included here. The contact rule "`name` or `lastName`" is not shown either: it is returned only as an error of [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md#required-fields) ||
|| **filterable**
[`boolean`](../../../data-types.md) | Whether the field can be used for filtering in the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| **sortable**
[`boolean`](../../../data-types.md) | Whether the field can be used for sorting in the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| **editable**
[`boolean`](../../../data-types.md) | Whether the field can be passed in the [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md) and [crm.&lt;type&gt;.update](../items/crm-entity-update-rest-v3.md) methods ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field \"foo\" found in entity \"CrmFieldMetadataDto\"."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `CrmFieldMetadataDto` | Check the names in `select` against the [description fields table](#item) ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid field in `select`: `#VALUE#` | Pass `select` as an array, for example, `["name"]`, not as a string ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a user with permission to read items of this object type ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.field.list`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-field-get-rest-v3.md)
- [{#T}](./crm-entity-field-custom-list-rest-v3.md)
- [{#T}](../items/crm-entity-add-rest-v3.md)
- [{#T}](../items/crm-entity-list-rest-v3.md)
