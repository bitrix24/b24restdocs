# Get a List of Custom Fields v 3.0 crm.[type].field.custom.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit CRM preferences" permission in CRM

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.field.custom.list` method returns the custom fields of an object type with their settings. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.field.custom.list` | [Deal fields](../items/fields-deal.md) ||
|| Lead | `crm.lead.field.custom.list` | [Lead fields](../items/fields-lead.md) ||
|| Contact | `crm.contact.field.custom.list` | [Contact fields](../items/fields-contact.md) ||
|| Company | `crm.company.field.custom.list` | [Company fields](../items/fields-company.md) ||
|| Estimate | `crm.quote.field.custom.list` | [Estimate fields](../items/fields-quote.md) ||
|| Invoice | `crm.smartinvoice.field.custom.list` | [Invoice fields](../items/fields-smart-invoice.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.field.custom.list`, for example, `crm.smartprocess1038.field.custom.list` | [SPA fields](../items/fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. SPAs have custom field methods regardless of the type settings. My company does not have these methods: Bitrix24 responds to `crm.mycompany.field.custom.list` with the ["Method Not Found"](#method-not-found) error.

The method name is written in lowercase: `crm.smartinvoice.field.custom.list`, not `crm.smartInvoice.field.custom.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of field parameters to return, for example, `["id", "name", "userTypeId"]`. You can pass `id`, `entityId`, and the parameters from the description of the [fields parameter](./crm-entity-field-custom-add-rest-v3.md#fields) of the create method.

By default, the method returns all field parameters ||
|| **filter**
[`array`](../../../data-types.md) | Field selection conditions. [Parameter description](#filter) ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"parameter": "asc"}` or `{"parameter": "desc"}`. You can sort by `id` and `name` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination: `limit` — the number of fields to return, `offset` — the number of fields to skip, for example, `{"limit": 20, "offset": 40}` ||
|#

### The filter Parameter {#filter}

An array of conditions in the form `["parameter", "operator", "value"]`, for example, `[["name", "=", "UF_CRM_CONTRACT_NUMBER"]]`. The condition format is described in the [REST 3.0 overview](../../../rest-v3.md#filter).

You can filter by these parameters:

- `id` — field ID
- `name` — field name
- `entityId` — object type code

The method does not filter by other parameters, for example, `userTypeId`, and returns a request validation error. If `filter` is passed as a string instead of an array, the method returns a filter error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.field.custom.list`

{% endnote %}

The examples retrieve the names, types, and labels of the custom fields of an SPA with the type ID `1038`, sorted by name. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","userTypeId","editFormLabel"],"order":{"name":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.field.custom.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","userTypeId","editFormLabel"],"order":{"name":"asc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.field.custom.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CustomFieldListResult = {
      items: Array<{
        id: number
        name: string
        userTypeId: string
        editFormLabel: string | null
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CustomFieldListResult>(
        'crm.smartprocess1038.field.custom.list',
        {
          select: ['id', 'name', 'userTypeId', 'editFormLabel'],
          order: {
            name: 'asc',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach((field) => console.info(field.id, field.name, field.userTypeId))
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
      async function listCustomFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.field.custom.list',
            {
              select: ['id', 'name', 'userTypeId', 'editFormLabel'],
              order: {
                name: 'asc',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach((field) => console.info(field.id, field.name, field.userTypeId))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listCustomFields)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.field.custom.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["id", "name", "userTypeId", "editFormLabel"],
                "order": {
                    "name": "asc",
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        for field in result["items"]:
            print(field["id"], field["name"], field["userTypeId"])
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
                'crm.smartprocess1038.field.custom.list',
                [
                    'select' => ['id', 'name', 'userTypeId', 'editFormLabel'],
                    'order' => [
                        'name' => 'asc',
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
        'crm.smartprocess1038.field.custom.list',
        {
            select: ['id', 'name', 'userTypeId', 'editFormLabel'],
            order: {
                name: 'asc',
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
        'crm.smartprocess1038.field.custom.list',
        [
            'select' => ['id', 'name', 'userTypeId', 'editFormLabel'],
            'order' => [
                'name' => 'asc',
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.field.custom.list", b24.Params{
    	"select": []string{"id", "name", "userTypeId", "editFormLabel"},
    	"order": b24.Params{
    		"name": "asc",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.field.custom.list: %w", err)
    }

    var out struct {
    	Items []struct {
    		ID   int    `json:"id"`
    		Name string `json:"name"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, f := range out.Items {
    	fmt.Println(f.ID, f.Name)
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
                "id": 6008205,
                "name": "UF_CRM_19_PRIORITY",
                "userTypeId": "enumeration",
                "editFormLabel": "Priority"
            },
            {
                "id": 6008209,
                "name": "UF_CRM_CONTRACT_NUMBER",
                "userTypeId": "string",
                "editFormLabel": "Client contract number"
            }
        ]
    },
    "time": {
        "start": 1793716018,
        "finish": 1793716018.213075,
        "duration": 0.2130749225616455,
        "processing": 0,
        "date_start": "2026-11-03T17:26:58+03:00",
        "date_finish": "2026-11-03T17:26:58+03:00",
        "operating_reset_at": 1793716560,
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
[`array`](../../../data-types.md) | Array of field descriptions with the parameters from `select`. Without `select`, all [field parameters](./crm-entity-field-custom-add-rest-v3.md#fields), `id`, and `entityId`. The array is empty if there are no fields or the user does not have the "Edit CRM preferences" permission in CRM ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "DTO \"CustomFieldConfigDto\" in field \"userTypeId\" requires attribute \"Filterable\" to perform this request.",
                "field": "userTypeId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `userTypeId` | DTO `CustomFieldConfigDto` in field `userTypeId` requires attribute `Filterable` to perform this request | Filter only by `id`, `name`, and `entityId` ||
|| `sort` | DTO `CustomFieldConfigDto` in field `sort` requires attribute `Sortable` to perform this request | Sort only by `id` and `name` ||
|#

#### Filter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example, `[["name", "=", "UF_CRM_CONTRACT_NUMBER"]]` ||
|#

#### Unknown Filter Operator

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown filter operator `%` | Use the operators from the [REST 3.0 overview](../../../rest-v3.md#filter) ||
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
|| `-` | Incorrect `pagination` parameter value: `#VALUE#` | Pass a `limit` greater than zero and an `offset` of zero or greater ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `CustomFieldConfigDto` | Check the names in `select`: the allowed names are the [field parameters](./crm-entity-field-custom-add-rest-v3.md#fields), `id`, and `entityId` ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid field in `select`: `#VALUE#` | Pass `select` as an array of names, for example, `["id", "name"]` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.field.custom.list`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. My company does not have these methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-field-custom-add-rest-v3.md)
- [{#T}](./crm-entity-field-custom-update-rest-v3.md)
- [{#T}](./crm-entity-field-custom-get-rest-v3.md)
- [{#T}](./crm-entity-field-custom-delete-rest-v3.md)
