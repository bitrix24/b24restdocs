# Get a Field Description v 3.0 crm.[type].field.get

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

The `crm.<type>.field.get` method returns the description of a single field of a CRM object type by name. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.field.get` | [Deal fields](../items/fields-deal.md) ||
|| Lead | `crm.lead.field.get` | [Lead fields](../items/fields-lead.md) ||
|| Contact | `crm.contact.field.get` | [Contact fields](../items/fields-contact.md) ||
|| Company | `crm.company.field.get` | [Company fields](../items/fields-company.md) ||
|| Estimate | `crm.quote.field.get` | [Estimate fields](../items/fields-quote.md) ||
|| Invoice | `crm.smartinvoice.field.get` | [Invoice fields](../items/fields-smart-invoice.md) ||
|| My company | `crm.mycompany.field.get` | [My company fields](../items/fields-my-company.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.field.get`, for example, `crm.smartprocess1038.field.get` | [SPA fields](../items/fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. SPAs have field description methods regardless of the type settings.

The method name is written in lowercase: `crm.smartinvoice.field.get`, not `crm.smartInvoice.field.get`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Field name in camelCase, for example, `stageId`, or a custom field name, for example, `UF_CRM_CONTRACT_NUMBER`. The method does not find standard fields written in uppercase, for example, `TITLE`. The list of names is returned by the [crm.&lt;type&gt;.field.list](./crm-entity-field-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of description fields to return, for example, `["name", "type", "editable"]`. [Description fields](#item).

By default, the method returns all description fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.field.get`

{% endnote %}

The examples retrieve the description of the stage field of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"stageId"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"stageId","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.field.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldGetResult = {
      item: {
        name: string
        type: string
        title: string
        multiple: boolean
        editable: boolean
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<FieldGetResult>(
        'crm.smartprocess1038.field.get',
        {
          name: 'stageId',
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.name, result.item.type, result.item.editable)
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
      async function getField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.field.get',
            {
              name: 'stageId',
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.name, result.item.type, result.item.editable)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getField)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.field.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "name": "stageId",
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["item"])
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
                'crm.smartprocess1038.field.get',
                [
                    'name' => 'stageId',
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
        'crm.smartprocess1038.field.get',
        {
            name: 'stageId',
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
        'crm.smartprocess1038.field.get',
        [
            'name' => 'stageId',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.field.get", b24.Params{
    	"name": "stageId",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.field.get: %w", err)
    }

    var out struct {
    	Item struct {
    		Name     string `json:"name"`
    		Type     string `json:"type"`
    		Editable bool   `json:"editable"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(out.Item.Name, out.Item.Type, out.Item.Editable)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "name": "stageId",
            "type": "string",
            "elementType": null,
            "multiple": false,
            "title": "Stage",
            "description": null,
            "validationRules": [],
            "requiredGroups": null,
            "filterable": true,
            "sortable": true,
            "editable": true
        }
    },
    "time": {
        "start": 1793715000,
        "finish": 1793715000.500155,
        "duration": 0.500154972076416,
        "processing": 0,
        "date_start": "2026-11-03T17:10:00+03:00",
        "date_finish": "2026-11-03T17:10:00+03:00",
        "operating_reset_at": 1793715600,
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
|| **result.item**
[`object`](../../../data-types.md) | Field description. [Description fields](#item) ||
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

HTTP status: **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION",
        "message": "Field \"TITLE\" was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is missing | Pass `name` ||
|| `name` | This request requires data type `string` in field `name` | Pass the field name as a string ||
|#

#### Field Not Found

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Field `#NAME#` was not found | Check the field name with the [crm.&lt;type&gt;.field.list](./crm-entity-field-list-rest-v3.md) method. Standard field names are written in camelCase: `title`, not `TITLE` ||
|#

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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.field.get`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-field-list-rest-v3.md)
- [{#T}](./crm-entity-field-custom-list-rest-v3.md)
