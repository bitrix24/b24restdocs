# Retrieve an Item's Emails v 3.0 crm.[type].activity.email.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the CRM item whose emails are requested

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

Retrieves emails bound to a single CRM item: their subject, direction, sender, recipients, and, on request, email text. The `<type>` segment in the method name specifies the object type; the `ownerId` filter condition specifies the item.

The list includes only emails: the method skips other kinds of activities, which do not take up space on the page. All activities of an item are returned by the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method.

## Method Name for the Object Type

#|
|| **Object Type** | **Method** ||
|| Deal | `crm.deal.activity.email.list` ||
|| Lead | `crm.lead.activity.email.list` ||
|| Contact | `crm.contact.activity.email.list` ||
|| Company | `crm.company.activity.email.list` ||
|| Estimate | `crm.quote.activity.email.list` ||
|| Invoice | `crm.smartinvoice.activity.email.list` ||
|| CRM Document | `crm.smartdocument.activity.email.list` ||
|| SPA | `crm.smartprocess<entityTypeId>.activity.email.list`, for example `crm.smartprocess1038.activity.email.list` ||
|#

For an SPA, substitute its type ID `entityTypeId` in the method name. SPA type IDs `entityTypeId` are returned by the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method.

The method name is written in lowercase: `crm.smartinvoice.activity.email.list`, rather than `crm.smartInvoice.activity.Email.list`. A name with different capitalization returns the [Method Not Found](#method-not-found) error.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Filter in REST 3.0 format: an array of conditions in the form `["field", "operator", "value"]`. The `["ownerId","=",<item ID>]` condition is required, for example, `[["ownerId","=",741],["direction","=","incoming"]]`. [Parameter Description](#filter) ||
|| **select**
[`array`](../../../data-types.md) | List of email fields to return, for example, `["id","subject","from","to"]`. Available fields are described in the [Item Object](./crm-entity-activity-email-get-rest-v3.md#item) table of the `crm.<type>.activity.email.get` method. For `bindings` and `communications`, you can select nested fields using dot notation: `communications.value`.

The email text is returned only if `body` or `isBodyTruncated` is specified in `select`. Without them, both fields return `null`.

Default: all email fields, without the email text ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. You can sort by `id`, `createdTime`, `updatedTime`, `startTime`, `endTime`, and `deadlineTime`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

Default: `{"id": "asc"}` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object. [Parameter Description](#pagination) ||
|#

### Parameter filter {#filter}

The `ownerId` condition specifies the item whose emails to retrieve. The following rules apply:

- a single condition at the top level of the filter, with the `=` operator and a positive integer
- `ownerId` cannot be specified in a condition group, under negation, with another operator, twice, or separately from `filter`
- if the item does not exist, is in the Recycle Bin, or you do not have permission to view it, the method returns a "not found" error rather than an empty list

Other conditions are combined with it using AND. You can filter by `id`, `providerId`, `isCompleted`, `assignedById`, `direction`, `createdTime`, `updatedTime`, `startTime`, and `deadlineTime`. REST 3.0 operators and condition groups are supported; see the [REST 3.0 Overview](../../../rest-v3.md#filter) for syntax. Write a condition group with the `"type": "filter"` key: `{"type":"filter","logic":"or","conditions":[...]}`.

The email fields `subject`, `from`, `to`, `cc`, `bcc`, `parentActivityId`, and `body` cannot be used for filtering or sorting. For example, to find emails from a specific address, retrieve the item's emails and filter the records by `from` on your side.

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of emails per page. Values greater than `200` are capped at `200`. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. If `page` is passed, `offset` is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of emails to skip from the beginning of the list.

Default: `0` ||
|#

{% note info "" %}

The response does not contain the total number of emails. If a page returns fewer emails than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.deal.activity.email.list`

{% endnote %}

The examples retrieve a deal's outgoing emails, newest first. For another object type, replace the method name using the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",741],["direction","=","outgoing"]],"select":["id","subject","from","to","createdTime","isCompleted"],"order":{"createdTime":"desc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.activity.email.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",741],["direction","=","outgoing"]],"select":["id","subject","from","to","createdTime","isCompleted"],"order":{"createdTime":"desc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.deal.activity.email.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EmailListResult = {
      items: Array<{
        id: number
        subject: string
        from: string | null
        to: string[] | null
        createdTime: string
        isCompleted: boolean
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<EmailListResult>(
        'crm.deal.activity.email.list',
        {
          filter: [
            ['ownerId', '=', 741],
            ['direction', '=', 'outgoing'],
          ],
          select: ['id', 'subject', 'from', 'to', 'createdTime', 'isCompleted'],
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
        console.info('Emails:', result.items)
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
      async function listEmails() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.deal.activity.email.list',
            {
              filter: [
                ['ownerId', '=', 741],
                ['direction', '=', 'outgoing'],
              ],
              select: ['id', 'subject', 'from', 'to', 'createdTime', 'isCompleted'],
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
          console.info('Emails:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listEmails)
    </script>
    ```

- Python

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.deal.activity.email.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["ownerId", "=", 741],
                    ["direction", "=", "outgoing"],
                ],
                "select": ["id", "subject", "from", "to", "createdTime", "isCompleted"],
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.activity.email.list',
                [
                    'filter' => [
                        ['ownerId', '=', 741],
                        ['direction', '=', 'outgoing'],
                    ],
                    'select' => ['id', 'subject', 'from', 'to', 'createdTime', 'isCompleted'],
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.deal.activity.email.list',
        {
            filter: [
                ['ownerId', '=', 741],
                ['direction', '=', 'outgoing'],
            ],
            select: ['id', 'subject', 'from', 'to', 'createdTime', 'isCompleted'],
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.activity.email.list',
        [
            'filter' => [
                ['ownerId', '=', 741],
                ['direction', '=', 'outgoing'],
            ],
            'select' => ['id', 'subject', 'from', 'to', 'createdTime', 'isCompleted'],
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
    // client and ctx have already been created — see the "SDK for Go" section
    res, err := client.Core().Call(ctx, "crm.deal.activity.email.list", b24.Params{
    	"filter": []any{
    		[]any{"ownerId", "=", 741},
    		[]any{"direction", "=", "outgoing"},
    	},
    	"select": []string{"id", "subject", "from", "to", "createdTime", "isCompleted"},
    	"order": b24.Params{
    		"createdTime": "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.deal.activity.email.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID          int      `json:"id"`
    		Subject     string   `json:"subject"`
    		From        *string  `json:"from"`
    		To          []string `json:"to"`
    		CreatedTime string   `json:"createdTime"`
    		IsCompleted bool     `json:"isCompleted"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.CreatedTime, item.Subject)
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
                "subject": "Re: Sales Proposal",
                "from": "sales@example.com",
                "to": [
                    "client@example.com"
                ],
                "id": 8697,
                "isCompleted": true,
                "createdTime": "2026-11-27T12:41:56+03:00"
            },
            {
                "subject": "Re: Sales Proposal",
                "from": "sales@example.com",
                "to": [
                    "client@example.com"
                ],
                "id": 8693,
                "isCompleted": true,
                "createdTime": "2026-11-27T12:41:29+03:00"
            },
            {
                "subject": "Sales Proposal",
                "from": "sales@example.com",
                "to": [
                    "client@example.com"
                ],
                "id": 1203,
                "isCompleted": true,
                "createdTime": "2020-12-30T14:02:28+03:00"
            }
        ]
    },
    "time": {
        "start": 1795772607,
        "finish": 1795772607.142473,
        "duration": 0.1424729824066162,
        "processing": 0,
        "date_start": "2026-11-27T12:43:27+03:00",
        "date_finish": "2026-11-27T12:43:27+03:00",
        "operating_reset_at": 1795773180,
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
[`object[]`](../../../data-types.md) | Array of emails with the fields specified in `select`, or all fields if `select` is omitted. The array is empty if the item has no matching emails. The fields are the same as in the [Item Object](./crm-entity-activity-email-get-rest-v3.md#item) of the `crm.<type>.activity.email.get` method ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

Differences from the `crm.<type>.activity.email.get` response:

- `body` and `isBodyTruncated` are populated only if either is specified in `select`. Otherwise, both fields return `null`
- if the email's customer is hidden from you by access settings, the email remains in the list, but `from`, `to`, `cc`, `bcc`, `parentActivityId`, `body`, and `isBodyTruncated` return `null`. The `crm.<type>.activity.email.get` method returns a "not found" error for such an email

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION",
        "message": "Error validating request object filters.",
        "validation": [
            {
                "message": "Filter for required field `ownerId` is required",
                "field": "filter.ownerId"
            }
        ]
    }
}
```

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter` | Required field `filter` is not specified | Pass `filter` with an `ownerId` condition. Passing `filter` as a string also causes this error ||
|| `ownerId` | ownerId is a condition of the filter: name it as ["ownerId", "=", N] there. | Remove `ownerId` from the request body and pass it as a condition in `filter` ||
|| `subject` | DTO `EmailActivityDto` in field `subject` requires attribute `Filterable` to perform this request | Filter only by `id`, `providerId`, `isCompleted`, `assignedById`, `direction`, `createdTime`, `updatedTime`, `startTime`, and `deadlineTime` ||
|| `subject` | DTO `EmailActivityDto` in field `subject` requires attribute `Sortable` to perform this request | Sort only by `id`, `createdTime`, `updatedTime`, `startTime`, `endTime`, and `deadlineTime` ||
|| `direction` | This request requires data type `Direction` in field `direction` | Pass a value from the `direction` field list: `incoming`, `outgoing`, `undefined` ||
|#

#### Filter Validation Errors

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter.ownerId` | Filter for required field `ownerId` is required | Add the `["ownerId","=",<item ID>]` condition to `filter` ||
|#

#### Invalid Filter

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Invalid filter `["ownerId",">",741]` | Check the `ownerId` condition: a single condition at the top level, with the `=` operator and a positive number. Write condition groups with the `"type": "filter"` key ||
|#

#### Invalid Pagination

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Item Not Found

Error code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Entity with ID `999999999` was not found | The item from `ownerId` does not exist for this object type, is in the Recycle Bin, or you do not have permission to view it. Check `ownerId` and the method name ||
|#

#### Unknown Field Error

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `description` found in entity `EmailActivityDto` | Check the field names in `select`, `filter`, and `order` against the [Item Object](./crm-entity-activity-email-get-rest-v3.md#item) table. The `type` and `description` fields and activity files are returned by the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method ||
|| `-` | Unknown field `foo` found in entity `ActivityCommunicationDto` | Check the nested field in `select`: `communications.type`, `communications.value`, `communications.entityTypeId`, `communications.entityId` ||
|#

#### Access Error

Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | For `crm.lead.activity.email.list`: leads are disabled in Bitrix24. Enable leads or request emails of another object type ||
|#

#### Method Not Found {#method-not-found}

Error code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Method `crm.mycompany.activity.email.list` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.activity.email.list`. For an SPA, check `entityTypeId` with the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method. My company and B2E documents have no email methods ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](./crm-entity-activity-email-get-rest-v3.md)
- [{#T}](./crm-entity-activity-email-send-rest-v3.md)
- [{#T}](./crm-activity-email-get-rest-v3.md)
- [{#T}](./crm-activity-email-reply-rest-v3.md)
