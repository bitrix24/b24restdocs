# Retrieve an Item’s Timeline Comments v 3.0 crm.[type].timeline.comment.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the CRM item; for my company, permission to view my companies is required

{% note info "" %}

This method is part of REST 3.0. The calling conventions and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

`crm.<type>.timeline.comment.list` retrieves timeline comments for a single CRM item. The `<type>` segment in the method name specifies the item type, and the `ownerId` filter condition specifies the item. Comments for multiple items cannot be retrieved in one call.

{% note warning "" %}

Without `select`, the method returns only the ID for each comment: `{"items":[{"id":77161},{"id":77145}]}`. To retrieve the text, author, and date, list the fields in `select`.

{% endnote %}

## Method Name by Object Type

#|
|| **Object Type** | **Method** ||
|| Deal | `crm.deal.timeline.comment.list` ||
|| Lead | `crm.lead.timeline.comment.list` ||
|| Contact | `crm.contact.timeline.comment.list` ||
|| Company | `crm.company.timeline.comment.list` ||
|| My company | `crm.mycompany.timeline.comment.list` ||
|| Estimate | `crm.quote.timeline.comment.list` ||
|| Invoice | `crm.smartinvoice.timeline.comment.list` ||
|| CRM document | `crm.smartdocument.timeline.comment.list` ||
|| B2E document | `crm.smartb2edocument.timeline.comment.list` ||
|| SPA | `crm.smartprocess<entityTypeId>.timeline.comment.list`, for example, `crm.smartprocess1038.timeline.comment.list` ||
|#

For an SPA, substitute its type ID `entityTypeId` into the method name. The [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method returns SPA type IDs `entityTypeId`.

Company and my company are different types: `crm.company.timeline.comment.list` cannot find my company, and `crm.mycompany.timeline.comment.list` cannot find a regular company.

The method name must be lowercase: `crm.smartinvoice.timeline.comment.list`, rather than `crm.smartInvoice.timeline.comment.list`. A name with different casing returns a [Method Not Found](#method-not-found) error.

## Method Parameters

{% include [Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | REST 3.0 filter with a single condition `["ownerId","=",<item ID>]`, for example, `[["ownerId","=",741]]`. The condition can also be passed without the outer array: `["ownerId","=",741]`. [Parameter Description](#filter) ||
|| **select**
[`array`](../../../data-types.md) | Comment fields to return, for example, `["id","text","authorId","createdTime"]`. [Comment Fields](./crm-entity-timeline-comment-get-rest-v3.md#item).

Default: only `id` ||
|| **order**
[`object`](../../../data-types.md) | Sort by creation date in the format `{"createdTime": "direction"}`. Other fields do not support sorting. Comments with the same date are sorted by `id` in the same direction.

Possible direction values:

- `asc` — oldest first
- `desc` — newest first

Default: `{"createdTime": "desc"}` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object. [Parameter Description](#pagination) ||
|#

### filter Parameter {#filter}

The filter accepts exactly one condition — `["ownerId","=",<item ID>]`:

- the value is a positive integer, not a string
- other fields, other operators such as `>` or `in`, a second condition, groups with `"logic": "or"`, and negation cause an error
- a filter passed as an object `{"ownerId":741}` causes an error

Retrieve the item ID using [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md).

If the item specified in `ownerId` does not exist, has been deleted, or you have no permission to view it, the method returns an [Item Not Found](#entity-not-found) error instead of an empty list. If the item has no comments, `items` is an empty array.

### pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of comments per page. Values greater than `1000` are capped at `1000`. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting at `1`. If `page` is passed, `offset` is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of comments to skip from the beginning of the list. A negative value causes an error.

Default: `0` ||
|#

{% note info "" %}

The response does not include the total number of comments. If a page contains fewer comments than specified in `limit`, it is the last page. A page number beyond the end of the list returns an empty `items` array.

{% endnote %}

## Code Examples

{% include [Examples](../../../../_includes/examples.md) %}

{% note info "" %}

Calls to the new API include the `/api/` segment in the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.deal.timeline.comment.list`

{% endnote %}

The examples retrieve deal comments, newest first. For another object type, replace the method name using the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",741]],"select":["id","ownerTypeId","ownerId","text","authorId","createdTime"],"order":{"createdTime":"desc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.timeline.comment.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",741]],"select":["id","ownerTypeId","ownerId","text","authorId","createdTime"],"order":{"createdTime":"desc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.deal.timeline.comment.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods supported by `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method using the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CommentListResult = {
      items: Array<{
        id: number
        ownerTypeId: number
        ownerId: number
        text: string
        authorId: number | null
        createdTime: string
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CommentListResult>(
        'crm.deal.timeline.comment.list',
        {
          filter: [
            ['ownerId', '=', 741],
          ],
          select: ['id', 'ownerTypeId', 'ownerId', 'text', 'authorId', 'createdTime'],
          order: {
            createdTime: 'desc',
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
        console.info('Comments:', result.items)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods supported by `$b24.actions.v3.call.make`. Call the method using the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listComments() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.deal.timeline.comment.list',
            {
              filter: [
                ['ownerId', '=', 741],
              ],
              select: ['id', 'ownerTypeId', 'ownerId', 'text', 'authorId', 'createdTime'],
              order: {
                createdTime: 'desc',
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
          console.info('Comments:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listComments)
    </script>
    ```

- Python

    CRM methods have no built-in wrappers in `b24pysdk`, and the SDK sends only methods from `Config().api_v3_methods` to REST 3.0. Add the method to this list and call it using `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.deal.timeline.comment.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["ownerId", "=", 741],
                ],
                "select": ["id", "ownerTypeId", "ownerId", "text", "authorId", "createdTime"],
                "order": {
                    "createdTime": "desc",
                },
                "pagination": {
                    "limit": 50,
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["items"])
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

    The SDK does not yet support calls to `/rest/api/`. Send a direct HTTP request, for example, using cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.timeline.comment.list',
                [
                    'filter' => [
                        ['ownerId', '=', 741],
                    ],
                    'select' => ['id', 'ownerTypeId', 'ownerId', 'text', 'authorId', 'createdTime'],
                    'order' => [
                        'createdTime' => 'desc',
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

    The SDK does not yet support calls to `/rest/api/`. Send a direct HTTP request, for example, using cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.deal.timeline.comment.list',
        {
            filter: [
                ['ownerId', '=', 741],
            ],
            select: ['id', 'ownerTypeId', 'ownerId', 'text', 'authorId', 'createdTime'],
            order: {
                createdTime: 'desc',
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

    The SDK does not yet support calls to `/rest/api/`. Send a direct HTTP request, for example, using cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.timeline.comment.list',
        [
            'filter' => [
                ['ownerId', '=', 741],
            ],
            'select' => ['id', 'ownerTypeId', 'ownerId', 'text', 'authorId', 'createdTime'],
            'order' => [
                'createdTime' => 'desc',
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
    // client and ctx have already been created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.deal.timeline.comment.list", b24.Params{
    	"filter": []any{
    		[]any{"ownerId", "=", 741},
    	},
    	"select": []string{"id", "ownerTypeId", "ownerId", "text", "authorId", "createdTime"},
    	"order": b24.Params{
    		"createdTime": "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.deal.timeline.comment.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID          int    `json:"id"`
    		OwnerTypeID int    `json:"ownerTypeId"`
    		OwnerID     int    `json:"ownerId"`
    		Text        string `json:"text"`
    		AuthorID    *int   `json:"authorId"`
    		CreatedTime string `json:"createdTime"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.CreatedTime, item.Text)
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
                "id": 77161,
                "ownerTypeId": 2,
                "ownerId": 741,
                "text": "Sent a commercial proposal, awaiting a response by December 1",
                "authorId": 1,
                "createdTime": "2026-11-27T12:40:05+03:00"
            },
            {
                "id": 77145,
                "ownerTypeId": 2,
                "ownerId": 741,
                "text": "The client confirmed [b]payment[/b] by Friday",
                "authorId": 1,
                "createdTime": "2026-11-27T12:36:56+03:00"
            }
        ]
    },
    "time": {
        "start": 1795772417,
        "finish": 1795772418.012379,
        "duration": 1.0123789310455322,
        "processing": 1,
        "date_start": "2026-11-27T12:40:17+03:00",
        "date_finish": "2026-11-27T12:40:18+03:00",
        "operating_reset_at": 1795773000,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Response data object ||
|| **result.items**
[`object[]`](../../../data-types.md) | Array of comments containing the fields from `select`, or only `id` if `select` is omitted. If there are no comments, the array is empty. The fields are described on the [crm.&lt;type&gt;.timeline.comment.get](./crm-entity-timeline-comment-get-rest-v3.md#item) page ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

The list contains only comments. The method does not return activities, emails, history entries, or other timeline records. Comment files are not included in the response; the previous version’s [crm.timeline.comment.list](../../timeline/comments/crm-timeline-comment-list.md) method returns them in `FILES`.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION",
        "message": "Error validating request object filters.",
        "validation": [
            {
                "message": "Filter for required field \"ownerId\" is required.",
                "field": "filter.ownerId"
            }
        ]
    }
}
```

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Missing Filter

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter.ownerId` | — | Pass `filter` with the condition `[["ownerId","=",<item ID>]]` ||
|#

#### Invalid Filter

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Pass a single condition with the `=` operator: `["ownerId","=",741]`. The `>`, `!=`, and `in` operators, a second condition, and condition groups are not supported ||
|| `-` | — | Pass the filter as an array `[["ownerId","=",741]]`, not an object `{"ownerId":741}` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `ownerId` | — | Pass the item ID as a positive number, not a string ||
|| `text` | — | Filter only by `ownerId` ||
|| `id` | — | Sort only by `createdTime` ||
|#

#### Invalid Sort Order

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Pass the `asc` or `desc` direction ||
|#

#### Invalid Pagination

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Pass a `limit` greater than zero and an `offset` of zero or greater ||
|#

#### Item Not Found {#entity-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | The item specified in `ownerId` does not exist for this object type, has been deleted, or you have no permission to view it. Check `ownerId` and the method name: my company and a regular company are different types ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Pass only fields from the [Comment Fields Table](./crm-entity-timeline-comment-get-rest-v3.md#item) in `select` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Leads are disabled in the CRM settings. This error is returned only by the lead method. Enable leads ||
|#

#### Feature Unavailable on the Plan

Error Code: `FEATURE_NOT_AVAILABLE_ON_CURRENT_PLAN`, HTTP status **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | The Bitrix24 plan does not include leads. This error is returned only by the lead method ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Check the method name: it must be lowercase, for example, `crm.smartinvoice.timeline.comment.list`. For an SPA, check `entityTypeId` using [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-timeline-comment-add-rest-v3.md)
- [{#T}](./crm-entity-timeline-comment-get-rest-v3.md)
- [{#T}](../../timeline/comments/crm-timeline-comment-list.md)
