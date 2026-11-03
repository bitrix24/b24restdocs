# Count CRM Items v 3.0 crm.[type].aggregate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view items of this CRM object type. For my company, a CRM administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.aggregate` method returns the number of CRM items that match a filter. The object type is set by the `<type>` segment of the method name.

The method counts the same items that [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) would return with the same filter: only items available to the user, without my companies in `crm.company.aggregate` and without recurring deal templates in `crm.deal.aggregate`.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.aggregate` | [Deal fields](./fields-deal.md) ||
|| Lead | `crm.lead.aggregate` | [Lead fields](./fields-lead.md) ||
|| Contact | `crm.contact.aggregate` | [Contact fields](./fields-contact.md) ||
|| Company | `crm.company.aggregate` | [Company fields](./fields-company.md) ||
|| Estimate | `crm.quote.aggregate` | [Estimate fields](./fields-quote.md) ||
|| Invoice | `crm.smartinvoice.aggregate` | [Invoice fields](./fields-smart-invoice.md) ||
|| My company | `crm.mycompany.aggregate` | [My company fields](./fields-my-company.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.aggregate`, for example, `crm.smartprocess1038.aggregate` | [SPA fields](./fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

The method name is written in lowercase: `crm.smartinvoice.aggregate`, not `crm.smartInvoice.aggregate`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select***
[`object`](../../../data-types.md) | Aggregate function and field. The method accepts only one value — `{"count": ["id"]}`, the number of items.

Other functions and fields, for example, `{"sum": ["opportunity"]}` or `{"count": ["title"]}`, cause the `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION` error, even though the OpenAPI description of the method lists the `avg`, `sum`, `min`, `max`, and `countDistinct` functions ||
|| **filter**
[`array`](../../../data-types.md) | Item selection conditions in the REST 3.0 format. The format, operators, condition groups, and limits are the same as for the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md#filter) method.

If the parameter is not passed, the method counts all available items ||
|#

The method does not accept the `order` and `pagination` parameters: if they are passed, it returns a sort or pagination error.

You can filter by fields whose `filterable` attribute is `true` in the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) response. Each object type has its own set of fields, which are described on these pages:

- [deal fields](./fields-deal.md)
- [lead fields](./fields-lead.md)
- [contact fields](./fields-contact.md)
- [company fields](./fields-company.md)
- [estimate fields](./fields-quote.md)
- [invoice fields](./fields-smart-invoice.md)
- [my company fields](./fields-my-company.md)
- [SPA fields](./fields-smart-process.md)

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.aggregate`

{% endnote %}

The examples count SPA items with the type ID `1038` in pipeline `69` with an amount of 10,000 or more. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":{"count":["id"]},"filter":[["categoryId","=",69],["opportunity",">=",10000]]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.aggregate
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":{"count":["id"]},"filter":[["categoryId","=",69],["opportunity",">=",10000]],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.aggregate
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ItemAggregateResult = {
      result: {
        count: {
          id: number
        }
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ItemAggregateResult>(
        'crm.smartprocess1038.aggregate',
        {
          select: {
            count: ['id'],
          },
          filter: [
            ['categoryId', '=', 69],
            ['opportunity', '>=', 10000],
          ],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Count:', result.result.count.id)
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
      async function countItems() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.aggregate',
            {
              select: {
                count: ['id'],
              },
              filter: [
                ['categoryId', '=', 69],
                ['opportunity', '>=', 10000],
              ],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Count:', result.result.count.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', countItems)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.aggregate"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": {"count": ["id"]},
                "filter": [
                    ["categoryId", "=", 69],
                    ["opportunity", ">=", 10000],
                ],
            },
            prefer_version=3,
        )
        count = response["result"]["result"]["count"]["id"]
        print(count)
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
                'crm.smartprocess1038.aggregate',
                [
                    'select' => [
                        'count' => ['id'],
                    ],
                    'filter' => [
                        ['categoryId', '=', 69],
                        ['opportunity', '>=', 10000],
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
        'crm.smartprocess1038.aggregate',
        {
            select: {
                count: ['id'],
            },
            filter: [
                ['categoryId', '=', 69],
                ['opportunity', '>=', 10000],
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
        'crm.smartprocess1038.aggregate',
        [
            'select' => [
                'count' => ['id'],
            ],
            'filter' => [
                ['categoryId', '=', 69],
                ['opportunity', '>=', 10000],
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.aggregate", b24.Params{
    	"select": b24.Params{
    		"count": []string{"id"},
    	},
    	"filter": []any{
    		[]any{"categoryId", "=", 69},
    		[]any{"opportunity", ">=", 10000},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.aggregate: %w", err)
    }

    var aggregate struct {
    	Result struct {
    		Count struct {
    			ID int `json:"id"`
    		} `json:"count"`
    	} `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &aggregate); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(aggregate.Result.Count.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": {
            "count": {
                "id": 3
            }
        }
    },
    "time": {
        "start": 1793706442,
        "finish": 1793706442.214798,
        "duration": 0.2147979736328125,
        "processing": 0,
        "date_start": "2026-11-03T14:47:22+03:00",
        "date_finish": "2026-11-03T14:47:22+03:00",
        "operating_reset_at": 1793707020,
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
|| **result.result**
[`object`](../../../data-types.md) | Aggregation results grouped by function ||
|| **result.result.count**
[`object`](../../../data-types.md) | Result of the `count` function for the fields from `select` ||
|| **result.result.count.id**
[`integer`](../../../data-types.md) | Number of items that match the filter. If no items match, `0` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION",
        "message": "Invalid field in \"select\": {\"sum\":[\"opportunity\"]}."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Required field `select` is missing | Pass `"select": {"count": ["id"]}` ||
|| `#FIELD#` | DTO `#DTO#` in field `#FIELD#` requires attribute `Filterable` to perform this request | The field cannot be used in a filter. Use fields whose `filterable` attribute is `true` in `crm.<type>.field.list` ||
|| `#FIELD#` | This request requires data type `#TYPE#` in field `#FIELD#` | Pass a value of the field type in the condition: a number, `true` or `false`, or a date and time with a time zone ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid field in `select`: #SELECT# | Pass `select` as the object `{"count": ["id"]}`. The method does not accept other functions, fields, or the array `["id"]` ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `#FILTER#` | Check the condition from the error message against the description of the [filter parameter](./crm-entity-list-rest-v3.md#filter). A common cause is a condition group inside an array without `"type": "filter"` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown filter operator `#OPERATOR#` | Use the operators from the [REST 3.0 overview](../../../rest-v3.md#operators) ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `#DTO#` | Check the field name in `filter` on the fields page of the object type ||
|#

#### Extra Parameters

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid sort order `#ORDER#` | Remove `order` from the request ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: #PAGINATION# | Remove `pagination` from the request ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.aggregate`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-get-rest-v3.md)
- [{#T}](./crm-entity-list-rest-v3.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
