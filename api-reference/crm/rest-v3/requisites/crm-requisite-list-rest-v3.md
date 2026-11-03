# Get a List of Bitrix24 Requisites v 3.0 crm.requisite.list

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

The `crm.requisite.list` method returns the requisites of all Bitrix24 contacts and companies, including my companies, with filtering and pagination. The method is suitable for exporting and searching requisites, for example, by TIN. The requisites of one contact or one company are returned by the [crm.&lt;type&gt;.requisite.list](./crm-entity-requisite-list-rest-v3.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`, condition groups with `"type": "filter"`. The syntax is described in the [REST 3.0 overview](../../../rest-v3.md#filter).

You can filter by `id`, `ownerId`, `ownerTypeId`, `presetId`, `active`, `xmlId`, and most requisite data fields: `inn`, `vatNumber`, `companyTitle`, `lastName`, and others. The full list is returned by `crm.requisite.field.list` with the `filterable` flag.

`ownerTypeId` is the owner type: `3` for a contact, `4` for a company. Other types cause an error ||
|| **select**
[`array`](../../../data-types.md) | List of requisite fields to return. The fields are the same as for a [requisite](./index.md#fields), but the owner is set by the `ownerTypeId` and `ownerId` pair instead of `companyId` or `contactId`. The method does not return the nested `addresses` and `bankRequisites`.

Default: `id` only ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. Sorting is available by `id` and `ownerId`.

Possible direction values:

- `asc` — ascending
- `desc` — descending ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination: `limit` is the number of requisites per page, `50` by default; `offset` is the number of requisites to skip ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.requisite.list`

{% endnote %}

The examples search for company requisites with the tax number `21/815/08150`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerTypeId","=",4],["inn","=","21/815/08150"]],"select":["id","ownerTypeId","ownerId","presetId","name","inn"],"order":{"id":"asc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerTypeId","=",4],["inn","=","21/815/08150"]],"select":["id","ownerTypeId","ownerId","presetId","name","inn"],"order":{"id":"asc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.requisite.list
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
        ownerTypeId: number
        ownerId: number
        presetId: number
        name: string
        inn: string | null
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<RequisiteListResult>(
        'crm.requisite.list',
        {
          filter: [
            ['ownerTypeId', '=', 4],
            ['inn', '=', '21/815/08150'],
          ],
          select: ['id', 'ownerTypeId', 'ownerId', 'presetId', 'name', 'inn'],
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
      async function listAllRequisites() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.requisite.list',
            {
              filter: [
                ['ownerTypeId', '=', 4],
                ['inn', '=', '21/815/08150'],
              ],
              select: ['id', 'ownerTypeId', 'ownerId', 'presetId', 'name', 'inn'],
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

      document.addEventListener('DOMContentLoaded', listAllRequisites)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.requisite.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["ownerTypeId", "=", 4],
                    ["inn", "=", "21/815/08150"],
                ],
                "select": ["id", "ownerTypeId", "ownerId", "presetId", "name", "inn"],
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
                'crm.requisite.list',
                [
                    'filter' => [
                        ['ownerTypeId', '=', 4],
                        ['inn', '=', '21/815/08150'],
                    ],
                    'select' => ['id', 'ownerTypeId', 'ownerId', 'presetId', 'name', 'inn'],
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
        'crm.requisite.list',
        {
            filter: [
                ['ownerTypeId', '=', 4],
                ['inn', '=', '21/815/08150'],
            ],
            select: ['id', 'ownerTypeId', 'ownerId', 'presetId', 'name', 'inn'],
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
        'crm.requisite.list',
        [
            'filter' => [
                ['ownerTypeId', '=', 4],
                ['inn', '=', '21/815/08150'],
            ],
            'select' => ['id', 'ownerTypeId', 'ownerId', 'presetId', 'name', 'inn'],
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
    res, err := client.Core().Call(ctx, "crm.requisite.list", b24.Params{
    	"filter": []any{
    		[]any{"ownerTypeId", "=", 4},
    		[]string{"inn", "=", "21/815/08150"},
    	},
    	"select": []string{"id", "ownerTypeId", "ownerId", "presetId", "name", "inn"},
    	"order": b24.Params{
    		"id": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.requisite.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID          int     `json:"id"`
    		OwnerTypeID int     `json:"ownerTypeId"`
    		OwnerID     int     `json:"ownerId"`
    		PresetID    int     `json:"presetId"`
    		Name        string  `json:"name"`
    		Inn         *string `json:"inn"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.OwnerTypeID, item.OwnerID, item.Name)
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
                "ownerTypeId": 4,
                "ownerId": 3167,
                "presetId": 1,
                "name": "Main requisite",
                "inn": "21/815/08150"
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
[`object[]`](../../../data-types.md) | Requisites with the fields from `select`. Without `select`, each requisite has only `id`. [Requisite field description](./index.md#fields) ||
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
                "message": "The method does not return details of owner type 2.",
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
|| `name` | DTO `RequisiteDto` in field `name` requires attribute `Filterable` to perform this request | Filter by fields with the `filterable` flag from `crm.requisite.field.list` ||
|| `name` | DTO `RequisiteDto` in field `name` requires attribute `Sortable` to perform this request | Sort only by `id` and `ownerId` ||
|#

#### Filter Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.ownerTypeId` | The method does not return details of owner type `#OWNER_TYPE_ID#` | Pass `3` for a contact or `4` for a company ||
|#

#### Unknown Operator

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown filter operator `#OPERATOR#` | Use the operators from the [REST 3.0 overview](../../../rest-v3.md#filter) ||
|#

#### Invalid Pagination

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect "pagination" parameter value: `#PAGINATION#` | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `RequisiteDto` | Check the field names. The owner is set by `ownerTypeId` and `ownerId`; the method has no nested `addresses` and `bankRequisites` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The method is available only to a Bitrix24 administrator ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, `crm.requisite.list` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-requisite-list-rest-v3.md)
- [{#T}](./crm-entity-requisite-get-rest-v3.md)
