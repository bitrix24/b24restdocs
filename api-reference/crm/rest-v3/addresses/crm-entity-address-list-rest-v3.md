# Get Contact or Company Addresses v 3.0 crm.[type].address.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the contact or company whose addresses are requested

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.address.list` method returns the addresses of one contact or company from all its requisites. The owner type is set by the `<type>` segment of the method name, and the owner is set by a filter condition.

The addresses of all contacts, companies, and leads in Bitrix24 are returned by the [crm.address.list](./crm-address-list-rest-v3.md) method.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Contact | `crm.contact.address.list` ||
|| Company | `crm.company.address.list` ||
|#

A lead address is returned by the [crm.lead.address.get](./crm-entity-address-get-rest-v3.md) method.

The method name is written in lowercase: `crm.company.address.list`, not `crm.Company.address.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions in the form `["field", "operator", "value"]`. The owner condition is required, for example, `[["contactId","=",3219]]`. [Parameter description](#filter) ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. You can sort by the `id` and `typeId` fields.

Possible direction values:

- `asc` — ascending
- `desc` — descending ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object: `limit` is the number of addresses per page, `50` by default, `page` is the page number, and `offset` is the number of addresses to skip. A zero or negative `limit` and a negative `offset` cause an error ||
|#

The method returns all address fields. The `select` parameter does not affect the response.

### The filter Parameter {#filter}

The method accepts only the conditions from the table and combines them with AND:

#|
|| **Field** | **Operators** | **Description** ||
|| `contactId` | `=` | Contact ID. A required condition in `crm.contact.address.list` ||
|| `companyId` | `=` | Company ID. A required condition in `crm.company.address.list` ||
|| `typeId` | `=` | Address type, for example, `6` — legal address. The values are listed in the [address methods overview](./index.md#fields) ||
|#

The method does not accept condition groups with `logic`, the `in` operator, other operators, or other fields.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.contact.address.list`

{% endnote %}

The examples retrieve all addresses of a contact. For a company, replace the method name according to the table above and the `contactId` condition with `companyId`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["contactId","=",3219]],"order":{"typeId":"asc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.address.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["contactId","=",3219]],"order":{"typeId":"asc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.contact.address.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type AddressListResult = {
      items: Array<{
        id: number
        typeId: number
        contactId: number
        requisiteId: number
        address1: string
        city: string
        postalCode: string
        country: string
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<AddressListResult>(
        'crm.contact.address.list',
        {
          filter: [
            ['contactId', '=', 3219],
          ],
          order: {
            typeId: 'asc',
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
        console.info('Addresses:', result.items)
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
      async function listAddresses() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.contact.address.list',
            {
              filter: [
                ['contactId', '=', 3219],
              ],
              order: {
                typeId: 'asc',
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
          console.info('Addresses:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listAddresses)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.contact.address.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["contactId", "=", 3219],
                ],
                "order": {
                    "typeId": "asc",
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
                'crm.contact.address.list',
                [
                    'filter' => [
                        ['contactId', '=', 3219],
                    ],
                    'order' => [
                        'typeId' => 'asc',
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
        'crm.contact.address.list',
        {
            filter: [
                ['contactId', '=', 3219],
            ],
            order: {
                typeId: 'asc',
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
        'crm.contact.address.list',
        [
            'filter' => [
                ['contactId', '=', 3219],
            ],
            'order' => [
                'typeId' => 'asc',
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
    res, err := client.Core().Call(ctx, "crm.contact.address.list", b24.Params{
    	"filter": []any{
    		[]any{"contactId", "=", 3219},
    	},
    	"order": b24.Params{
    		"typeId": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.contact.address.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID          int    `json:"id"`
    		TypeID      int    `json:"typeId"`
    		ContactID   int    `json:"contactId"`
    		RequisiteID int    `json:"requisiteId"`
    		Address1    string `json:"address1"`
    		City        string `json:"city"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.TypeID, item.City, item.Address1)
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
                "id": 633,
                "typeId": 1,
                "contactId": 3219,
                "requisiteId": 547,
                "address1": "Friedrichstraße 1",
                "address2": "Office 10",
                "city": "Berlin",
                "postalCode": "10117",
                "region": "",
                "province": "",
                "country": "Germany",
                "countryCode": ""
            },
            {
                "id": 637,
                "typeId": 6,
                "contactId": 3219,
                "requisiteId": 547,
                "address1": "Hauptstraße 5",
                "address2": "",
                "city": "Munich",
                "postalCode": "",
                "region": "",
                "province": "",
                "country": "",
                "countryCode": ""
            }
        ]
    },
    "time": {
        "start": 1793709949,
        "finish": 1793709949.853585,
        "duration": 0.8535850048065186,
        "processing": 0,
        "date_start": "2026-11-03T15:45:49+03:00",
        "date_finish": "2026-11-03T15:45:49+03:00",
        "operating_reset_at": 1793710260,
        "operating": 0.3442521095275879
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
[`object[]`](../../../data-types.md) | Owner addresses with all fields. Company addresses contain `companyId` instead of `contactId`. If there are no addresses, the array is empty. [Description of address fields](./index.md#fields) ||
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
                "message": "Filter for required field \"contactId\" is required.",
                "field": "filter.contactId"
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
|| `city` | DTO `OwnerAddressContactDto` in field `city` requires attribute `Filterable` to perform this request | Filter only by the owner and `typeId` ||
|| `city` | DTO `OwnerAddressContactDto` in field `city` requires attribute `Sortable` to perform this request | Sort only by `id` and `typeId` ||
|#

#### Filter Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.contactId` | Filter for required field `contactId` is required | Add an owner condition to `filter`. For a company, the field is `companyId` ||
|#

#### Invalid Filter

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `#FILTER#` | Use only the `=` operator and the conditions from the [filter parameter](#filter) table ||
|#

#### Invalid Pagination

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `#PAGE#` | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Owner Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | The contact or company from the filter does not exist, or you do not have permission to view it. Check the ID and the method name ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.company.address.list`. The method exists only for contacts and companies ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-address-get-rest-v3.md)
- [{#T}](./crm-entity-address-add-rest-v3.md)
- [{#T}](./crm-address-list-rest-v3.md)
