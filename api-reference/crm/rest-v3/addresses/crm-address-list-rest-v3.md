# Get Contact, Company, and Lead Addresses v 3.0 crm.address.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a Bitrix24 administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.address.list` method returns the addresses of all contacts, companies, and leads in Bitrix24. The method is intended for exporting addresses: it does not check permissions for each owner, so only a Bitrix24 administrator can call it. CRM administrator permission is not enough for this method.

The addresses of one contact or company are returned by the [crm.&lt;type&gt;.address.list](./crm-entity-address-list-rest-v3.md) method, and a lead address by [crm.lead.address.get](./crm-entity-address-get-rest-v3.md).

The method also returns addresses whose owners no longer exist in Bitrix24. Addresses of owners in the Recycle Bin are not returned.

The `crm.address.list` method at the `/rest/api/` address is a new REST 3.0 method. The previous [crm.address.list](../../requisites/addresses/crm-address-list.md) at the `/rest/` address works as before and has different fields and filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of address fields to return, for example, `["id","ownerTypeId","ownerId","city"]`. [Field description](#fields).

Default: `id` only ||
|| **filter**
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions in the form `["field", "operator", "value"]`, for example, `[["ownerTypeId","=",3]]`. You can filter by the `id`, `typeId`, `ownerTypeId`, and `ownerId` fields.

The syntax and operators are described in the [REST 3.0 overview](../../../rest-v3.md#filter) ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. You can sort only by `id`.

Possible direction values:

- `asc` — ascending
- `desc` — descending ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object: `limit` is the number of addresses per page, `50` by default, `page` is the page number, and `offset` is the number of addresses to skip ||
|#

### Address Fields {#fields}

#|
|| **Field**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Address ID. When the address type changes, the address gets a new ID ||
|| **typeId**
[`integer`](../../../data-types.md) | Address type: `1` — street address, `4` — registered address, `6` — legal address, `9` — beneficiary address, `11` — delivery address. The list of types is returned by [crm.dictionary.addresstype.list](../dictionaries/crm-dictionary-list-rest-v3.md) ||
|| **ownerTypeId**
[`integer`](../../../data-types.md) | Owner type: `1` — lead, `3` — contact, `4` — company. Other values in the filter cause an error ||
|| **ownerId**
[`integer`](../../../data-types.md) | Owner ID. It is unique only together with `ownerTypeId`: a filter by `ownerId` alone returns the addresses of a lead, a contact, and a company with this number ||
|| **requisiteId**
[`integer`](../../../data-types.md) | ID of the requisite in which a contact or company address is stored. `0` for a lead address ||
|| **address1**
[`string`](../../../data-types.md) | Street, building, block, structure ||
|| **address2**
[`string`](../../../data-types.md) | Apartment or office ||
|| **city**
[`string`](../../../data-types.md) | City ||
|| **postalCode**
[`string`](../../../data-types.md) | Postal code ||
|| **region**
[`string`](../../../data-types.md) | District ||
|| **province**
[`string`](../../../data-types.md) | Region ||
|| **country**
[`string`](../../../data-types.md) | Country ||
|| **countryCode**
[`string`](../../../data-types.md) | Country code from the Bitrix24 locations reference ||
|#

Empty text fields are returned as an empty string. The field description is returned by the `crm.address.field.list` and `crm.address.field.get` methods.

### How to Export All Addresses

For a full export, page through by ID rather than by page number: sort by `{"id": "asc"}` and pass the condition `["id", ">", <last retrieved id>]` in each next request. With a large `offset`, the request runs slower.

An address whose type changes during the export gets a new `id` and may appear in the export twice or not at all.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.address.list`

{% endnote %}

The examples retrieve the next page of contact addresses after the address with `id = 100`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerTypeId","=",3],["id",">",100]],"select":["id","typeId","ownerTypeId","ownerId","requisiteId","address1","city"],"order":{"id":"asc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.address.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerTypeId","=",3],["id",">",100]],"select":["id","typeId","ownerTypeId","ownerId","requisiteId","address1","city"],"order":{"id":"asc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.address.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PortalAddressListResult = {
      items: Array<{
        id: number
        typeId: number
        ownerTypeId: number
        ownerId: number
        requisiteId: number
        address1: string
        city: string
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<PortalAddressListResult>(
        'crm.address.list',
        {
          filter: [
            ['ownerTypeId', '=', 3],
            ['id', '>', 100],
          ],
          select: ['id', 'typeId', 'ownerTypeId', 'ownerId', 'requisiteId', 'address1', 'city'],
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
      async function listPortalAddresses() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.address.list',
            {
              filter: [
                ['ownerTypeId', '=', 3],
                ['id', '>', 100],
              ],
              select: ['id', 'typeId', 'ownerTypeId', 'ownerId', 'requisiteId', 'address1', 'city'],
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
          console.info('Addresses:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listPortalAddresses)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.address.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["ownerTypeId", "=", 3],
                    ["id", ">", 100],
                ],
                "select": ["id", "typeId", "ownerTypeId", "ownerId", "requisiteId", "address1", "city"],
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
                'crm.address.list',
                [
                    'filter' => [
                        ['ownerTypeId', '=', 3],
                        ['id', '>', 100],
                    ],
                    'select' => ['id', 'typeId', 'ownerTypeId', 'ownerId', 'requisiteId', 'address1', 'city'],
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
        'crm.address.list',
        {
            filter: [
                ['ownerTypeId', '=', 3],
                ['id', '>', 100],
            ],
            select: ['id', 'typeId', 'ownerTypeId', 'ownerId', 'requisiteId', 'address1', 'city'],
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
        'crm.address.list',
        [
            'filter' => [
                ['ownerTypeId', '=', 3],
                ['id', '>', 100],
            ],
            'select' => ['id', 'typeId', 'ownerTypeId', 'ownerId', 'requisiteId', 'address1', 'city'],
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
    res, err := client.Core().Call(ctx, "crm.address.list", b24.Params{
    	"filter": []any{
    		[]any{"ownerTypeId", "=", 3},
    		[]any{"id", ">", 100},
    	},
    	"select": []string{"id", "typeId", "ownerTypeId", "ownerId", "requisiteId", "address1", "city"},
    	"order": b24.Params{
    		"id": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.address.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID          int    `json:"id"`
    		TypeID      int    `json:"typeId"`
    		OwnerTypeID int    `json:"ownerTypeId"`
    		OwnerID     int    `json:"ownerId"`
    		RequisiteID int    `json:"requisiteId"`
    		Address1    string `json:"address1"`
    		City        string `json:"city"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.OwnerTypeID, item.OwnerID, item.City)
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
                "id": 101,
                "typeId": 1,
                "ownerTypeId": 3,
                "ownerId": 113,
                "requisiteId": 0,
                "address1": "",
                "city": ""
            },
            {
                "id": 103,
                "typeId": 1,
                "ownerTypeId": 3,
                "ownerId": 115,
                "requisiteId": 0,
                "address1": "",
                "city": ""
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
[`object[]`](../../../data-types.md) | Addresses with the fields from `select`. Without `select`, each address contains only `id`. [Field description](#fields) ||
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
                "message": "Owner type 8 is not supported.",
                "field": "filter.ownerTypeId"
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
|| `city` | DTO `PortalAddressDto` in field `city` requires attribute `Filterable` to perform this request | Filter only by `id`, `typeId`, `ownerTypeId`, and `ownerId` ||
|| `typeId` | DTO `PortalAddressDto` in field `typeId` requires attribute `Sortable` to perform this request | Sort only by `id` ||
|#

#### Filter Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.ownerTypeId` | Owner type `#OWNER_TYPE_ID#` is not supported | Pass `1` — lead, `3` — contact, or `4` — company ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a Bitrix24 administrator ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `PortalAddressDto` | Check the field names in `select`, `filter`, and `order` against the [address fields](#fields) table. Here, the owner fields are named `ownerTypeId` and `ownerId`, not `contactId` or `companyId` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-address-list-rest-v3.md)
- [{#T}](./crm-entity-address-get-rest-v3.md)
