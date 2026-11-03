# Get a List of My Companies with Requisites and Contacts v 3.0 crm.mycompany.basic.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view companies, invoices, CRM documents, or B2E documents — permission for any one of these object types is enough

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.mycompany.basic.list` method returns a list of my companies with their basic data: the name, phone numbers, email addresses, requisites, and bank details.

The method is available without the CRM administrator permissions required by the [crm.mycompany.list](./crm-entity-list-rest-v3.md) method. For example, an employee who works only with invoices or documents can retrieve the list of their companies.

## Differences from crm.mycompany.list

#|
|| **Compared** | **crm.mycompany.basic.list** | **crm.mycompany.list** ||
|| Who can call | A user with permission to view companies, invoices, CRM documents, or B2E documents | CRM administrator ||
|| Fields | Only `id`, `title`, `email`, `phone`, `requisites`, `bankRequisites` | All my company fields except requisites, including custom fields. [My company fields](./fields-my-company.md) ||
|| Response without `select` | `id` and `title` | Only `id` ||
|| Filter | Only the `=` and `in` operators, the `id`, `title`, `email.value`, `phone.value` fields. No negation | Operators and fields from the [crm.&lt;type&gt;.list description](./crm-entity-list-rest-v3.md#filter) ||
|| Sorting | Only by `id` and `title` | By fields with the `sortable` attribute ||
|#

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of fields to return for each company. [Parameter description](#select)

Default: `["id", "title"]` ||
|| **filter**
[`array`](../../../data-types.md) | Conditions for selecting companies. [Parameter description](#filter)

If the parameter is not passed, the method returns all my companies ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. [Parameter description](#order)

Default: `{"id": "ASC"}` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters: `limit`, `page`, `offset`. [Parameter description](#pagination)

By default, the method returns the first 50 companies ||
|#

### The select Parameter {#select}

Available fields:

- `id` — company ID
- `title` — company name
- `email` — email addresses
- `phone` — phone numbers
- `requisites` — company requisites
- `bankRequisites` — company bank details

Fields of nested objects can be selected using dot notation: `phone.value`, `requisites.inn`, `bankRequisites.bankName`. The whole object name, for example, `requisites`, returns all of its fields.

Other fields, for example, `createdTime`, cause the `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` error. Descriptions of the available fields are returned by the `crm.mycompany.basic.field.list` and `crm.mycompany.basic.field.get` methods.

### The filter Parameter {#filter}

The filter is an array of conditions in the REST 3.0 format. Conditions are combined with AND logic:

- `["field", value]` — exact match; for an array value, inclusion in the list
- `["field", "=", value]` — exact match
- `["field", "in", [value, value]]` — inclusion in the list

You can filter by these fields:

- `id` — company ID
- `title` — company name, exact match
- `email.value` — email address
- `phone.value` — phone number

Other operators, for example, `>=` or `!=`, cause the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error. Conditions on requisites, for example, `requisites.inn`, cause the same error.

Conditions can be combined with OR logic using the `{"logic": "or", "conditions": [...]}` group. A group with `"negative": true` causes the `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION` error. For more information about condition groups, see the description of the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md#filter) method.

The filter searches only among my companies: a condition on the ID of a regular company returns an empty list.

### The order Parameter {#order}

An object where the key is `id` or `title`, and the value is the sort direction.

Possible direction values:

- `ASC` — ascending
- `DESC` — descending

If `id` is not in `order`, the method adds it as the last key with the same direction as the last field. Sorting by other fields causes an error.

### The pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of companies per page. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit` ||
|| **offset**
[`integer`](../../../data-types.md) | Number of companies to skip from the beginning of the list. A negative value causes an error.

Default: `0` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.mycompany.basic.list`

{% endnote %}

The examples retrieve two of my companies by ID with their phone numbers, email addresses, requisites, and bank names. The companies are sorted by name.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","phone","email","requisites.id","requisites.presetId","bankRequisites.id","bankRequisites.bankName"],"filter":[["id","in",[3,79]]],"order":{"title":"ASC"},"pagination":{"limit":10}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.mycompany.basic.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","phone","email","requisites.id","requisites.presetId","bankRequisites.id","bankRequisites.bankName"],"filter":[["id","in",[3,79]]],"order":{"title":"ASC"},"pagination":{"limit":10},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.mycompany.basic.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type Multifield = {
      id: number
      valueTypeId: string
      value: string
    }

    type MyCompanyBasicListResult = {
      items: {
        id: number
        title: string
        phone: Multifield[]
        email: Multifield[]
        requisites: { id: number, presetId: number }[]
        bankRequisites: { id: number, bankName: string | null }[]
      }[]
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<MyCompanyBasicListResult>(
        'crm.mycompany.basic.list',
        {
          select: [
            'id',
            'title',
            'phone',
            'email',
            'requisites.id',
            'requisites.presetId',
            'bankRequisites.id',
            'bankRequisites.bankName',
          ],
          filter: [
            ['id', 'in', [3, 79]],
          ],
          order: {
            title: 'ASC',
          },
          pagination: {
            limit: 10,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        for (const company of response.getData()!.result.items) {
          console.info(company.id, company.title, company.requisites.length)
        }
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
      async function listMyCompanies() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.mycompany.basic.list',
            {
              select: [
                'id',
                'title',
                'phone',
                'email',
                'requisites.id',
                'requisites.presetId',
                'bankRequisites.id',
                'bankRequisites.bankName',
              ],
              filter: [
                ['id', 'in', [3, 79]],
              ],
              order: {
                title: 'ASC',
              },
              pagination: {
                limit: 10,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          for (const company of response.getData().result.items) {
            console.info(company.id, company.title, company.requisites.length)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listMyCompanies)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.mycompany.basic.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": [
                    "id",
                    "title",
                    "phone",
                    "email",
                    "requisites.id",
                    "requisites.presetId",
                    "bankRequisites.id",
                    "bankRequisites.bankName",
                ],
                "filter": [
                    ["id", "in", [3, 79]],
                ],
                "order": {
                    "title": "ASC",
                },
                "pagination": {
                    "limit": 10,
                },
            },
            prefer_version=3,
        )
        for company in response["result"]["items"]:
            print(company["id"], company["title"], len(company["requisites"]))
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
                'crm.mycompany.basic.list',
                [
                    'select' => [
                        'id',
                        'title',
                        'phone',
                        'email',
                        'requisites.id',
                        'requisites.presetId',
                        'bankRequisites.id',
                        'bankRequisites.bankName',
                    ],
                    'filter' => [
                        ['id', 'in', [3, 79]],
                    ],
                    'order' => [
                        'title' => 'ASC',
                    ],
                    'pagination' => [
                        'limit' => 10,
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
        'crm.mycompany.basic.list',
        {
            select: [
                'id',
                'title',
                'phone',
                'email',
                'requisites.id',
                'requisites.presetId',
                'bankRequisites.id',
                'bankRequisites.bankName',
            ],
            filter: [
                ['id', 'in', [3, 79]],
            ],
            order: {
                title: 'ASC',
            },
            pagination: {
                limit: 10,
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
        'crm.mycompany.basic.list',
        [
            'select' => [
                'id',
                'title',
                'phone',
                'email',
                'requisites.id',
                'requisites.presetId',
                'bankRequisites.id',
                'bankRequisites.bankName',
            ],
            'filter' => [
                ['id', 'in', [3, 79]],
            ],
            'order' => [
                'title' => 'ASC',
            ],
            'pagination' => [
                'limit' => 10,
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
    res, err := client.Core().Call(ctx, "crm.mycompany.basic.list", b24.Params{
    	"select": []string{
    		"id", "title", "phone", "email",
    		"requisites.id", "requisites.presetId",
    		"bankRequisites.id", "bankRequisites.bankName",
    	},
    	"filter": []any{
    		[]any{"id", "in", []int{3, 79}},
    	},
    	"order": b24.Params{
    		"title": "ASC",
    	},
    	"pagination": b24.Params{
    		"limit": 10,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.mycompany.basic.list: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("no items key in the response")
    }

    var companies []struct {
    	ID         int    `json:"id"`
    	Title      string `json:"title"`
    	Requisites []struct {
    		ID       int `json:"id"`
    		PresetID int `json:"presetId"`
    	} `json:"requisites"`
    }
    if err := json.Unmarshal(raw, &companies); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, company := range companies {
    	fmt.Println(company.ID, company.Title, len(company.Requisites))
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
                "id": 3,
                "title": "Müller GmbH",
                "email": [
                    {
                        "id": 7605,
                        "valueTypeId": "WORK",
                        "valueType": {
                            "id": "WORK",
                            "name": "Work E-mail",
                            "nameShort": "Work"
                        },
                        "value": "info@example.com"
                    }
                ],
                "phone": [
                    {
                        "id": 7637,
                        "valueTypeId": "WORK",
                        "valueType": {
                            "id": "WORK",
                            "name": "Work Phone",
                            "nameShort": "Work Phone"
                        },
                        "value": "+49300000000"
                    }
                ],
                "requisites": [
                    {
                        "id": 3,
                        "presetId": 1
                    },
                    {
                        "id": 49,
                        "presetId": 1
                    }
                ],
                "bankRequisites": [
                    {
                        "id": 3,
                        "bankName": "Main account"
                    }
                ]
            },
            {
                "id": 79,
                "title": "Schneider GmbH",
                "email": [],
                "phone": [],
                "requisites": [
                    {
                        "id": 97,
                        "presetId": 1
                    }
                ],
                "bankRequisites": []
            }
        ]
    },
    "time": {
        "start": 1793707015,
        "finish": 1793707015.500961,
        "duration": 0.5009610652923584,
        "processing": 0,
        "date_start": "2026-11-03T14:56:55+03:00",
        "date_finish": "2026-11-03T14:56:55+03:00",
        "operating_reset_at": 1793707560,
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
[`array`](../../../data-types.md) | List of my companies. Each company contains only the fields from `select`. The company fields are described in the [crm.mycompany.basic.get](./crm-my-company-basic-get-rest-v3.md#item) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

The response does not contain the total number of companies. If a page returns fewer companies than specified in `limit`, it is the last page.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION",
        "message": "Invalid filter \"[\"id\",\">=\",3]\"."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Filter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `#CONDITION#` | Use only the `=` and `in` operators, the `id`, `title`, `email.value`, `phone.value` fields, and groups without `negative` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `BasicCompanyDto` | Pass in `select`, `filter`, and `order` only the fields from the parameter descriptions ||
|#

#### Sort Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid sort order `#DIRECTION#` | Pass `order` as an object, for example, `{"title":"ASC"}`, with the `ASC` or `DESC` direction ||
|#

#### Field Not Sortable

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | DTO `BasicCompanyDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Sort only by `id` or `title` ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: #PAGINATION# | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user has no permission to view companies, invoices, CRM documents, or B2E documents ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, `crm.mycompany.basic.list`, not `crm.myCompany.basic.list` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-my-company-basic-get-rest-v3.md)
- [{#T}](./crm-entity-list-rest-v3.md)
- [{#T}](./fields-my-company.md)
- [{#T}](../requisites/crm-requisite-list-rest-v3.md)
