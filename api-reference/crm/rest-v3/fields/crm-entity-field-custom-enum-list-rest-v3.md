# Get List Field Options v 3.0 crm.[type].field.custom.enum.list

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

The `crm.<type>.field.custom.enum.list` method returns the options of a single custom list field. The object type is set by the `<type>` segment of the method name.

A list field stores a set of options from which the user selects a value, for example, the priorities "High", "Medium", and "Low". The list field itself cannot be created with the [crm.&lt;type&gt;.field.custom.add](./crm-entity-field-custom-add-rest-v3.md) method: it does not accept the `enumeration` type. The option methods [crm.&lt;type&gt;.field.custom.enum.*](./crm-entity-field-custom-enum-list-rest-v3.md) work with list fields created in the Bitrix24 interface or in other ways. List fields are present in the response of the [crm.&lt;type&gt;.field.custom.list](./crm-entity-field-custom-list-rest-v3.md) method: their `userTypeId` is `enumeration`.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.field.custom.enum.list` | [Deal fields](../items/fields-deal.md) ||
|| Lead | `crm.lead.field.custom.enum.list` | [Lead fields](../items/fields-lead.md) ||
|| Contact | `crm.contact.field.custom.enum.list` | [Contact fields](../items/fields-contact.md) ||
|| Company | `crm.company.field.custom.enum.list` | [Company fields](../items/fields-company.md) ||
|| Estimate | `crm.quote.field.custom.enum.list` | [Estimate fields](../items/fields-quote.md) ||
|| Invoice | `crm.smartinvoice.field.custom.enum.list` | [Invoice fields](../items/fields-smart-invoice.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.field.custom.enum.list`, for example, `crm.smartprocess1038.field.custom.enum.list` | [SPA fields](../items/fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. SPAs have list field option methods regardless of the type settings. My company does not have these methods: for `crm.mycompany.field.custom.enum.list`, Bitrix24 returns the ["Method Not Found"](#method-not-found) error.

The method name is written in lowercase: `crm.smartinvoice.field.custom.enum.list`, not `crm.smartInvoice.field.custom.enum.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Condition on the list field. [Parameter description](#filter) ||
|| **select**
[`array`](../../../data-types.md) | List of option fields to return, for example, `["id", "value"]`. [Option fields](#item).

By default, the method returns all fields ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "asc"}` or `{"field": "desc"}`. You can sort by `id` and `sort` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination: `limit` — how many options to return, `offset` — how many to skip ||
|#

### The filter Parameter {#filter}

The filter consists of a single condition on the `fieldId` field, without an outer array: `["fieldId", "=", 6008205]`. Instead of the field ID, you can pass its name: `["fieldId", "=", "UF_CRM_19_PRIORITY"]`. Field IDs and names are returned by the [crm.&lt;type&gt;.field.custom.list](./crm-entity-field-custom-list-rest-v3.md) method.

For a user without the "Edit CRM preferences" permission, the [crm.&lt;type&gt;.field.custom.list](./crm-entity-field-custom-list-rest-v3.md) method does not return fields. Such a user can retrieve the list field name from the response of the [crm.&lt;type&gt;.field.list](./crm-entity-field-list-rest-v3.md) method: there the list field is described with the `int` type.

{% note warning "" %}

The method does not accept the array of conditions `[["fieldId", "=", 6008205]]` from the [REST 3.0 overview](../../../rest-v3.md#filter) and returns the error "Filter for required field `fieldId` is required".

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.field.custom.enum.list`

{% endnote %}

The examples retrieve the options of the "Priority" field of an SPA with the type ID `1038` in ascending order of `sort`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":["fieldId","=",6008205],"order":{"sort":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.field.custom.enum.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":["fieldId","=",6008205],"order":{"sort":"asc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.field.custom.enum.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EnumListResult = {
      items: Array<{
        id: number
        fieldId: number
        value: string
        isDefault: boolean
        sort: number
        xmlId: string
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<EnumListResult>(
        'crm.smartprocess1038.field.custom.enum.list',
        {
          filter: ['fieldId', '=', 6008205],
          order: {
            sort: 'asc',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach((value) => console.info(value.id, value.value, value.isDefault))
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
      async function listEnumValues() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.field.custom.enum.list',
            {
              filter: ['fieldId', '=', 6008205],
              order: {
                sort: 'asc',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach((value) => console.info(value.id, value.value, value.isDefault))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listEnumValues)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.field.custom.enum.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": ["fieldId", "=", 6008205],
                "order": {
                    "sort": "asc",
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        for value in result["items"]:
            print(value["id"], value["value"], value["isDefault"])
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
                'crm.smartprocess1038.field.custom.enum.list',
                [
                    'filter' => ['fieldId', '=', 6008205],
                    'order' => [
                        'sort' => 'asc',
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.field.custom.enum.list',
        {
            filter: ['fieldId', '=', 6008205],
            order: {
                sort: 'asc',
            },
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
        'crm.smartprocess1038.field.custom.enum.list',
        [
            'filter' => ['fieldId', '=', 6008205],
            'order' => [
                'sort' => 'asc',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.field.custom.enum.list", b24.Params{
    	"filter": []any{"fieldId", "=", 6008205},
    	"order": b24.Params{
    		"sort": "asc",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.field.custom.enum.list: %w", err)
    }

    var out struct {
    	Items []struct {
    		ID    int    `json:"id"`
    		Value string `json:"value"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, v := range out.Items {
    	fmt.Println(v.ID, v.Value)
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
                "id": 4135,
                "fieldId": 6008205,
                "value": "Very urgent",
                "isDefault": false,
                "sort": 1,
                "xmlId": "URGENT"
            },
            {
                "id": 4129,
                "fieldId": 6008205,
                "value": "High",
                "isDefault": false,
                "sort": 10,
                "xmlId": "HIGH"
            },
            {
                "id": 4131,
                "fieldId": 6008205,
                "value": "Medium",
                "isDefault": true,
                "sort": 20,
                "xmlId": "MEDIUM"
            },
            {
                "id": 4133,
                "fieldId": 6008205,
                "value": "Low",
                "isDefault": false,
                "sort": 30,
                "xmlId": "LOW"
            }
        ]
    },
    "time": {
        "start": 1793714992,
        "finish": 1793714992.390954,
        "duration": 0.39095401763916016,
        "processing": 0,
        "date_start": "2026-11-03T17:09:52+03:00",
        "date_finish": "2026-11-03T17:09:52+03:00",
        "operating_reset_at": 1793715540,
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
[`array`](../../../data-types.md) | Array of options. [Option fields](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Option Fields {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Option ID ||
|| **fieldId**
[`integer`](../../../data-types.md) | List field ID ||
|| **value**
[`string`](../../../data-types.md) | Option text ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Whether the option is selected by default ||
|| **sort**
[`integer`](../../../data-types.md) | Option position in the list ||
|| **xmlId**
[`string`](../../../data-types.md) | Option external code ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION",
        "message": "Error validating request object filters.",
        "validation": [
            {
                "message": "Filter for required field \"fieldId\" is required.",
                "field": "filter.fieldId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Missing Field Filter

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.fieldId` | Filter for required field `fieldId` is required | Pass the condition without an outer array: `"filter": ["fieldId", "=", 6008205]` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `value` | DTO `EnumDto` in field `value` requires attribute `Filterable` to perform this request | Filter only by `fieldId` ||
|| `value` | DTO `EnumDto` in field `value` requires attribute `Sortable` to perform this request | Sort only by `id` and `sort` ||
|#

#### Filter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of three elements, not as an object ||
|#

#### Field Not Found

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Field `#ID#` was not found | Check the field ID with the [crm.&lt;type&gt;.field.custom.list](./crm-entity-field-custom-list-rest-v3.md) method. The same error occurs if the field belongs to a different object type or is not a list field ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `EnumDto` | Check the names in `select` against the [option fields table](#item) ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid field in `select`: `#VALUE#` | Pass `select` as an array of names, for example, `["id", "value"]` ||
|#

#### Unknown Filter Operator

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown filter operator `%` | Use the `=` operator in the condition on `fieldId` ||
|#

#### Sort Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid sort order `#VALUE#` | Pass the `asc` or `desc` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `#VALUE#` | Pass a `limit` greater than zero ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.field.custom.enum.list`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. My company does not have these methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-field-custom-enum-add-rest-v3.md)
- [{#T}](./crm-entity-field-custom-enum-update-rest-v3.md)
- [{#T}](./crm-entity-field-custom-enum-get-rest-v3.md)
- [{#T}](./crm-entity-field-custom-enum-delete-rest-v3.md)
