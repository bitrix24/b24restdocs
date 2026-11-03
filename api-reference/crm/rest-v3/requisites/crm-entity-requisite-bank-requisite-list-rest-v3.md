# Get Bank Details of a Requisite v 3.0 crm.[type].requisite.bankrequisite.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the contact or company that owns the requisite

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.requisite.bankrequisite.list` method retrieves the bank details of one requisite of a contact or company. The bank details of all requisites of a contact or company are returned by the [crm.&lt;type&gt;.bankrequisite.list](./crm-entity-bank-requisite-list-rest-v3.md) method.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Company | `crm.company.requisite.bankrequisite.list` ||
|| Contact | `crm.contact.requisite.bankrequisite.list` ||
|#

Bank details belong to requisites of contacts and companies. The `company` or `contact` segment of the method name must match the owner of the requisite: a `crm.contact.*` method does not find a bank detail of a company.

The method name is written in lowercase: `crm.company.requisite.bankrequisite.list`, not `crm.company.requisite.bankRequisite.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Condition on the requisite: `["requisiteId","=",<ID>]`. The only operator is `=`, the value is a positive integer ||
|| **select**
[`array`](../../../data-types.md) | List of bank detail fields to return. [Field descriptions](./index.md#bank-fields). The `companyId` and `contactId` fields are not available in `select`.

Default: all fields available for the country of the requisite template ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. You can sort by `id`, `name`, and bank fields, except `requisiteId`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

Default: ascending by `id` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination: `limit` — number of records per page, `offset` — number of records to skip. Zero and negative values cause an error ||
|#

{% note warning "" %}

Pass `filter` as a single condition without an outer array: `["requisiteId","=",549]`. The standard REST 3.0 notation `[["requisiteId","=",549]]` currently fails validation and returns the `Filter for required field "requisiteId" is required.` error, even though the condition is present. For the same reason, you cannot add other filter conditions to the `requisiteId` condition.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.company.requisite.bankrequisite.list`

{% endnote %}

The examples retrieve the bank details of the company requisite `549`. For a contact, replace the method name with `crm.contact.requisite.bankrequisite.list`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":["requisiteId","=",549],"select":["id","requisiteId","name","bankName","iban","bic"],"order":{"name":"asc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.requisite.bankrequisite.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":["requisiteId","=",549],"select":["id","requisiteId","name","bankName","iban","bic"],"order":{"name":"asc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.company.requisite.bankrequisite.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type BankRequisiteListResult = {
      items: Array<{
        id: number
        requisiteId: number
        name: string
        bankName: string | null
        iban: string | null
        bic: string | null
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<BankRequisiteListResult>(
        'crm.company.requisite.bankrequisite.list',
        {
          filter: ['requisiteId', '=', 549],
          select: ['id', 'requisiteId', 'name', 'bankName', 'iban', 'bic'],
          order: {
            name: 'asc',
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
        console.info('Bank requisites:', result.items)
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
      async function listBankRequisites() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.company.requisite.bankrequisite.list',
            {
              filter: ['requisiteId', '=', 549],
              select: ['id', 'requisiteId', 'name', 'bankName', 'iban', 'bic'],
              order: {
                name: 'asc',
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
          console.info('Bank requisites:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listBankRequisites)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.company.requisite.bankrequisite.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": ["requisiteId", "=", 549],
                "select": ["id", "requisiteId", "name", "bankName", "iban", "bic"],
                "order": {
                    "name": "asc",
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
                'crm.company.requisite.bankrequisite.list',
                [
                    'filter' => ['requisiteId', '=', 549],
                    'select' => ['id', 'requisiteId', 'name', 'bankName', 'iban', 'bic'],
                    'order' => [
                        'name' => 'asc',
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
        'crm.company.requisite.bankrequisite.list',
        {
            filter: ['requisiteId', '=', 549],
            select: ['id', 'requisiteId', 'name', 'bankName', 'iban', 'bic'],
            order: {
                name: 'asc',
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
        'crm.company.requisite.bankrequisite.list',
        [
            'filter' => ['requisiteId', '=', 549],
            'select' => ['id', 'requisiteId', 'name', 'bankName', 'iban', 'bic'],
            'order' => [
                'name' => 'asc',
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
    res, err := client.Core().Call(ctx, "crm.company.requisite.bankrequisite.list", b24.Params{
    	"filter": []any{"requisiteId", "=", 549},
    	"select": []string{"id", "requisiteId", "name", "bankName", "iban", "bic"},
    	"order": b24.Params{
    		"name": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.company.requisite.bankrequisite.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID          int     `json:"id"`
    		RequisiteID int     `json:"requisiteId"`
    		Name        string  `json:"name"`
    		BankName    *string `json:"bankName"`
    		Iban        *string `json:"iban"`
    		Bic         *string `json:"bic"`
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
                "id": 23,
                "requisiteId": 549,
                "name": "Main account",
                "bankName": "Berliner Handelsbank AG",
                "iban": "DE89370400440532013000",
                "bic": "COBADEFFXXX"
            },
            {
                "id": 25,
                "requisiteId": 549,
                "name": "Reserve account",
                "bankName": "Rheinland Bank AG",
                "iban": "DE02370501980001802057",
                "bic": null
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
[`object[]`](../../../data-types.md) | Bank details with the fields from `select`. If there are no bank details, the array is empty. [Field descriptions](./index.md#bank-fields) ||
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
                "message": "Filter for required field \"requisiteId\" is required.",
                "field": "filter.requisiteId"
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
|| `filter` | Field value must not be empty | Pass `filter` with a condition on `requisiteId` ||
|| `requisiteId` | DTO `BankRequisiteListFilterDto` in field `requisiteId` requires attribute `Sortable` to perform this request | Remove the field from `order` ||
|#

#### Filter Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.requisiteId` | Filter for required field `requisiteId` is required | Pass the filter as a single condition without an outer array: `["requisiteId","=",<ID>]`. The error is also returned for the `[["requisiteId","=",…]]` notation and for operators other than `=` ||
|#

#### Invalid select

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid field in `select`: `companyId` | Remove `companyId` and `contactId` from `select` ||
|#

#### Invalid Pagination

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass `limit` greater than zero and `offset` of zero or greater ||
|#

#### Requisite Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | The requisite does not exist, belongs to an owner of a different type, or you do not have permission to view the owner. Check `requisiteId` and the method name ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.company.requisite.bankrequisite.list`. Requisite methods exist only for contacts and companies: the `company` or `contact` segment ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-bank-requisite-list-rest-v3.md)
- [{#T}](./crm-entity-requisite-bank-requisite-get-rest-v3.md)
- [{#T}](./crm-entity-requisite-bank-requisite-add-rest-v3.md)
