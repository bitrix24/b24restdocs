# Get Requisites and Contacts of My Company v 3.0 crm.mycompany.basic.get

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

The `crm.mycompany.basic.get` method returns the basic data of my company by ID: the name, phone numbers, email addresses, requisites, and bank details.

The method is available without the CRM administrator permissions required by the [crm.mycompany.get](./crm-entity-get-rest-v3.md) method. For example, an employee who works only with invoices or documents can retrieve the data of their company.

## Differences from crm.mycompany.get

#|
|| **Compared** | **crm.mycompany.basic.get** | **crm.mycompany.get** ||
|| Who can call | A user with permission to view companies, invoices, CRM documents, or B2E documents | CRM administrator ||
|| Fields | Only `id`, `title`, `email`, `phone`, `requisites`, `bankRequisites` | All my company fields except requisites, including custom fields. [My company fields](./fields-my-company.md) ||
|| Requisites | Requisites and bank details in the response | No. The requisites of my company are returned by the [crm.requisite.list](../requisites/crm-requisite-list-rest-v3.md) method with a filter by `ownerId` ||
|| Response without `select` | `id` and `title` | Only `id` ||
|| Company not found | HTTP status 404 | HTTP status 400 ||
|#

The method returns only companies from the "My companies" list. The ID of a regular company causes the `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | ID of my company. IDs can be retrieved with the [crm.mycompany.basic.list](./crm-my-company-basic-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of fields to return. [Parameter description](#select)

Default: `["id", "title"]` ||
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

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.mycompany.basic.get`

{% endnote %}

The examples retrieve the name, phone numbers, email addresses, the tax ID (INN) from the requisites, and the bank names of my company with ID `3`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3,"select":["id","title","phone","email","requisites.id","requisites.presetId","requisites.companyTitle","requisites.inn","bankRequisites.id","bankRequisites.bankName"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.mycompany.basic.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3,"select":["id","title","phone","email","requisites.id","requisites.presetId","requisites.companyTitle","requisites.inn","bankRequisites.id","bankRequisites.bankName"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.mycompany.basic.get
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

    type MyCompanyBasicGetResult = {
      item: {
        id: number
        title: string
        phone: Multifield[]
        email: Multifield[]
        requisites: { id: number, presetId: number, companyTitle: string, inn: string }[]
        bankRequisites: { id: number, bankName: string | null }[]
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<MyCompanyBasicGetResult>(
        'crm.mycompany.basic.get',
        {
          id: 3,
          select: [
            'id',
            'title',
            'phone',
            'email',
            'requisites.id',
            'requisites.presetId',
            'requisites.companyTitle',
            'requisites.inn',
            'bankRequisites.id',
            'bankRequisites.bankName',
          ],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const company = response.getData()!.result.item
        console.info(company.title, company.requisites.map(requisite => requisite.inn))
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
      async function getMyCompany() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.mycompany.basic.get',
            {
              id: 3,
              select: [
                'id',
                'title',
                'phone',
                'email',
                'requisites.id',
                'requisites.presetId',
                'requisites.companyTitle',
                'requisites.inn',
                'bankRequisites.id',
                'bankRequisites.bankName',
              ],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const company = response.getData().result.item
          console.info(company.title, company.requisites.map(requisite => requisite.inn))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMyCompany)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.mycompany.basic.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 3,
                "select": [
                    "id",
                    "title",
                    "phone",
                    "email",
                    "requisites.id",
                    "requisites.presetId",
                    "requisites.companyTitle",
                    "requisites.inn",
                    "bankRequisites.id",
                    "bankRequisites.bankName",
                ],
            },
            prefer_version=3,
        )
        company = response["result"]["item"]
        print(company["title"], [requisite["inn"] for requisite in company["requisites"]])
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
                'crm.mycompany.basic.get',
                [
                    'id' => 3,
                    'select' => [
                        'id',
                        'title',
                        'phone',
                        'email',
                        'requisites.id',
                        'requisites.presetId',
                        'requisites.companyTitle',
                        'requisites.inn',
                        'bankRequisites.id',
                        'bankRequisites.bankName',
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
        'crm.mycompany.basic.get',
        {
            id: 3,
            select: [
                'id',
                'title',
                'phone',
                'email',
                'requisites.id',
                'requisites.presetId',
                'requisites.companyTitle',
                'requisites.inn',
                'bankRequisites.id',
                'bankRequisites.bankName',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.mycompany.basic.get',
        [
            'id' => 3,
            'select' => [
                'id',
                'title',
                'phone',
                'email',
                'requisites.id',
                'requisites.presetId',
                'requisites.companyTitle',
                'requisites.inn',
                'bankRequisites.id',
                'bankRequisites.bankName',
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
    res, err := client.Core().Call(ctx, "crm.mycompany.basic.get", b24.Params{
    	"id": 3,
    	"select": []string{
    		"id", "title", "phone", "email",
    		"requisites.id", "requisites.presetId", "requisites.companyTitle", "requisites.inn",
    		"bankRequisites.id", "bankRequisites.bankName",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.mycompany.basic.get: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var company struct {
    	ID         int    `json:"id"`
    	Title      string `json:"title"`
    	Requisites []struct {
    		ID           int    `json:"id"`
    		CompanyTitle string `json:"companyTitle"`
    		Inn          string `json:"inn"`
    	} `json:"requisites"`
    }
    if err := json.Unmarshal(raw, &company); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(company.ID, company.Title, len(company.Requisites))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
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
                    "presetId": 1,
                    "companyTitle": "Müller GmbH",
                    "inn": "7700000000"
                },
                {
                    "id": 49,
                    "presetId": 1,
                    "companyTitle": "Müller Service GmbH",
                    "inn": "7700000001"
                }
            ],
            "bankRequisites": [
                {
                    "id": 3,
                    "bankName": "Main account"
                }
            ]
        }
    },
    "time": {
        "start": 1793706809,
        "finish": 1793706809.06065,
        "duration": 0.06065011024475098,
        "processing": 0,
        "date_start": "2026-11-03T14:53:29+03:00",
        "date_finish": "2026-11-03T14:53:29+03:00",
        "operating_reset_at": 1793707380,
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
[`object`](../../../data-types.md) | Data of my company. Contains only the fields from `select`. [Object description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### The item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Company ID ||
|| **title**
[`string`](../../../data-types.md) | Company name ||
|| **email**
[`array`](../../../data-types.md) | Email addresses. [Element description](#multifield). If there are no addresses, an empty array ||
|| **phone**
[`array`](../../../data-types.md) | Phone numbers. [Element description](#multifield). If there are no phone numbers, an empty array ||
|| **requisites**
[`array`](../../../data-types.md) | Company requisites. The requisite fields are the same as those returned by the [crm.requisite.list](../requisites/crm-requisite-list-rest-v3.md) method: `id`, `ownerId`, `ownerTypeId`, `presetId`, `name`, `companyTitle`, `inn`, `kpp`, `ogrn`, `directorName`, and others. If there are no requisites, an empty array ||
|| **bankRequisites**
[`array`](../../../data-types.md) | Bank details of all company requisites. [Element description](#bank-requisite). If there are no bank details, an empty array ||
|#

#### The email and phone Element {#multifield}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Value ID ||
|| **valueTypeId**
[`string`](../../../data-types.md) | Value type, for example, `WORK` ||
|| **valueType**
[`object`](../../../data-types.md) | Value type: `id` — code, `name` — full name, `nameShort` — short name ||
|| **value**
[`string`](../../../data-types.md) | Email address or phone number ||
|#

#### The bankRequisites Element {#bank-requisite}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Bank details ID ||
|| **requisiteId**
[`integer`](../../../data-types.md) | ID of the requisite the bank details belong to ||
|| **name**
[`string`](../../../data-types.md) | Bank details name ||
|| **bankName**
[`string`](../../../data-types.md) | Bank name ||
|| **bankAddress**
[`string`](../../../data-types.md) | Bank address ||
|| **bik**
[`string`](../../../data-types.md) | BIC ||
|| **bankAccountNumber**
[`string`](../../../data-types.md) | Account number ||
|| **bankAccountCurrency**
[`string`](../../../data-types.md) | Account currency ||
|| **correspondentAccountNumber**
[`string`](../../../data-types.md) | Correspondent account ||
|| **swift**
[`string`](../../../data-types.md) | SWIFT ||
|| **comments**
[`string`](../../../data-types.md) | Comment ||
|#

Bank details fields that are not filled in are returned as `null`. For more information about bank details, see the description of the [crm.&lt;type&gt;.requisite.bankrequisite.get](../requisites/crm-entity-requisite-bank-requisite-get-rest-v3.md) method.

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 1 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Company Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID #ID# was not found | There is no company with this `id`, or it is not in the "My companies" list. The IDs of my companies are returned by the [crm.mycompany.basic.list](./crm-my-company-basic-list-rest-v3.md) method ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the company `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `BasicCompanyDto` | Pass in `select` only the fields from the [parameter description](#select) ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, `crm.mycompany.basic.get`, not `crm.myCompany.basic.get` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-my-company-basic-list-rest-v3.md)
- [{#T}](./crm-entity-get-rest-v3.md)
- [{#T}](./fields-my-company.md)
- [{#T}](../requisites/crm-requisite-list-rest-v3.md)
