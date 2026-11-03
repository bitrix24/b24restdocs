# Get a List of Contact or Company Requisites v 3.0 crm.[type].requisite.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the contact or company

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.requisite.list` method returns the requisites of one contact or one company. The owner type is set by the `<type>` segment of the method name, and the owner by a filter condition. The requisites of all contacts and companies are returned by the [crm.requisite.list](./crm-requisite-list-rest-v3.md) method.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Company | `crm.company.requisite.list` ||
|| Contact | `crm.contact.requisite.list` ||
|#

Only contacts and companies have requisites. The owner type is set by the `company` or `contact` segment of the method name; the method does not find a requisite of a different owner type. My company requisites are available through the `crm.company.*` methods.

The method name is written in lowercase: `crm.company.requisite.list`, not `crm.Company.Requisite.List`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Filter with one owner condition: `[["companyId","=",3167]]` in the company method, `[["contactId","=",14]]` in the contact method. No other filter fields are available; the only operator is `=`, without groups or negation ||
|| **select**
[`array`](../../../data-types.md) | List of requisite fields to return. The fields and the nested `addresses` and `bankRequisites` objects are the same as in [select of the crm.&lt;type&gt;.requisite.get method](./crm-entity-requisite-get-rest-v3.md).

Default: `id` only ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. Sorting is available only by `id`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

Default: ascending by `id` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination: `limit` is the number of requisites per page, `50` by default; `offset` is the number of requisites to skip. A zero or negative `limit` causes an error ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.company.requisite.list`

{% endnote %}

The examples retrieve the requisites of the company with the ID `3167`. For a contact, replace the method name with `crm.contact.requisite.list` and `companyId` with `contactId`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["companyId","=",3167]],"select":["id","companyId","presetId","name","inn","vatNumber"],"order":{"id":"asc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.requisite.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["companyId","=",3167]],"select":["id","companyId","presetId","name","inn","vatNumber"],"order":{"id":"asc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.company.requisite.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RequisiteListResult = {
      items: Array<{
        id: number
        companyId: number
        presetId: number
        name: string
        inn: string | null
        vatNumber: string | null
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<RequisiteListResult>(
        'crm.company.requisite.list',
        {
          filter: [
            ['companyId', '=', 3167],
          ],
          select: ['id', 'companyId', 'presetId', 'name', 'inn', 'vatNumber'],
          order: {
            id: 'asc',
          },
          pagination: {
            limit: 50,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Requisites:', result.items)
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
      async function listRequisites() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.company.requisite.list',
            {
              filter: [
                ['companyId', '=', 3167],
              ],
              select: ['id', 'companyId', 'presetId', 'name', 'inn', 'vatNumber'],
              order: {
                id: 'asc',
              },
              pagination: {
                limit: 50,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Requisites:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listRequisites)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.company.requisite.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["companyId", "=", 3167],
                ],
                "select": ["id", "companyId", "presetId", "name", "inn", "vatNumber"],
                "order": {
                    "id": "asc",
                },
                "pagination": {
                    "limit": 50,
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
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
                'crm.company.requisite.list',
                [
                    'filter' => [
                        ['companyId', '=', 3167],
                    ],
                    'select' => ['id', 'companyId', 'presetId', 'name', 'inn', 'vatNumber'],
                    'order' => [
                        'id' => 'asc',
                    ],
                    'pagination' => [
                        'limit' => 50,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.company.requisite.list',
        {
            filter: [
                ['companyId', '=', 3167],
            ],
            select: ['id', 'companyId', 'presetId', 'name', 'inn', 'vatNumber'],
            order: {
                id: 'asc',
            },
            pagination: {
                limit: 50,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.company.requisite.list',
        [
            'filter' => [
                ['companyId', '=', 3167],
            ],
            'select' => ['id', 'companyId', 'presetId', 'name', 'inn', 'vatNumber'],
            'order' => [
                'id' => 'asc',
            ],
            'pagination' => [
                'limit' => 50,
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
    res, err := client.Core().Call(ctx, "crm.company.requisite.list", b24.Params{
    	"filter": []any{
    		[]any{"companyId", "=", 3167},
    	},
    	"select": []string{"id", "companyId", "presetId", "name", "inn", "vatNumber"},
    	"order": b24.Params{
    		"id": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.company.requisite.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID        int     `json:"id"`
    		CompanyID int     `json:"companyId"`
    		PresetID  int     `json:"presetId"`
    		Name      string  `json:"name"`
    		Inn       *string `json:"inn"`
    		VatNumber *string `json:"vatNumber"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Name)
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
                "id": 549,
                "companyId": 3167,
                "presetId": 1,
                "name": "Main requisite",
                "inn": "21/815/08150",
                "vatNumber": "DE987654321"
            },
            {
                "id": 551,
                "companyId": 3167,
                "presetId": 5,
                "name": "Private person requisite",
                "inn": null,
                "vatNumber": null
            }
        ]
    },
    "time": {
        "start": 1793711900,
        "finish": 1793711900.089311,
        "duration": 0.08931088447570801,
        "processing": 0,
        "date_start": "2026-11-03T16:18:20+03:00",
        "date_finish": "2026-11-03T16:18:20+03:00",
        "operating_reset_at": 1793712480,
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
[`object[]`](../../../data-types.md) | Owner requisites with the fields from `select`. Without `select`, each requisite has only `id`. If there are no requisites, the array is empty. [Requisite field description](./index.md#fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
                "message": "Filter for required field \"companyId\" is required.",
                "field": "filter.companyId"
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
|| `filter` | Required field `filter` is missing | Pass `filter` with an owner condition ||
|| `name` | DTO `OwnerRequisiteCompanyDto` in field `name` requires attribute `Filterable` to perform this request | Filter only by the owner ||
|| `sort` | DTO `OwnerRequisiteCompanyDto` in field `sort` requires attribute `Sortable` to perform this request | Sort only by `id` ||
|#

#### Filter Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.companyId` | Filter for required field `companyId` is required | Add the condition `["companyId","=",<ID>]`. In the contact method, use `contactId` ||
|| `filter.companyId` | Specify a single condition for the companyId field with the = operator, without nested groups or negation | Keep one owner condition with the `=` operator. The `in` and `>` operators and repeated conditions are not supported ||
|#

#### Invalid Pagination

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect "pagination" parameter value: `#PAGINATION#` | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Owner Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID #ID# was not found | The contact or company does not exist, or you have no permission to view it. Check the ID in the filter and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `OwnerRequisiteCompanyDto` | Check the field names in `select` against the [requisite field description](./index.md#fields) ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.company.requisite.list`. Requisite methods exist only for contacts and companies: the `company` or `contact` segment ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-requisite-get-rest-v3.md)
- [{#T}](./crm-requisite-list-rest-v3.md)
- [{#T}](./crm-entity-requisite-add-rest-v3.md)
