# Retrieve a Timeline Comment v 3.0 crm.[type].timeline.comment.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the CRM item linked to the comment; for my company, permission to view my companies is required

{% note info "" %}

This method is part of REST 3.0. The calling conventions and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

`crm.<type>.timeline.comment.get` retrieves a timeline comment by ID. The `<type>` segment in the method name specifies the object type: the comment is found only if it is linked to an item of this type. The `crm.lead.timeline.comment.get` method cannot find a deal comment.

{% note warning "" %}

Without `select`, the method returns only the comment ID: `{"item":{"id":77161}}`. To retrieve the text, author, and date, list the fields in `select`.

{% endnote %}

## Method Name by Object Type

#|
|| **Object Type** | **Method** ||
|| Deal | `crm.deal.timeline.comment.get` ||
|| Lead | `crm.lead.timeline.comment.get` ||
|| Contact | `crm.contact.timeline.comment.get` ||
|| Company | `crm.company.timeline.comment.get` ||
|| My company | `crm.mycompany.timeline.comment.get` ||
|| Estimate | `crm.quote.timeline.comment.get` ||
|| Invoice | `crm.smartinvoice.timeline.comment.get` ||
|| CRM document | `crm.smartdocument.timeline.comment.get` ||
|| B2E document | `crm.smartb2edocument.timeline.comment.get` ||
|| SPA | `crm.smartprocess<entityTypeId>.timeline.comment.get`, for example, `crm.smartprocess1038.timeline.comment.get` ||
|#

For an SPA, substitute its type ID `entityTypeId` into the method name. The [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method returns SPA type IDs `entityTypeId`.

The method name must be lowercase: `crm.smartinvoice.timeline.comment.get`, rather than `crm.smartInvoice.timeline.comment.get`. A name with different casing returns a [Method Not Found](#method-not-found) error.

## Method Parameters

{% include [Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Comment ID.

The ID is returned by [crm.&lt;type&gt;.timeline.comment.add](./crm-entity-timeline-comment-add-rest-v3.md) and [crm.&lt;type&gt;.timeline.comment.list](./crm-entity-timeline-comment-list-rest-v3.md) ||
|| **select**
[`array`](../../../data-types.md) | Comment fields to return, for example, `["id","text","authorId","createdTime"]`. [Comment Fields](#item).

Default: only `id` ||
|#

## Code Examples

{% include [Examples](../../../../_includes/examples.md) %}

{% note info "" %}

Calls to the new API include the `/api/` segment in the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.deal.timeline.comment.get`

{% endnote %}

The examples retrieve a deal comment. For another object type, replace the method name using the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":77161,"select":["id","ownerTypeId","ownerId","text","authorId","createdTime"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.timeline.comment.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":77161,"select":["id","ownerTypeId","ownerId","text","authorId","createdTime"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.deal.timeline.comment.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods supported by `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method using the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CommentGetResult = {
      item: {
        id: number
        ownerTypeId: number
        ownerId: number
        text: string
        authorId: number | null
        createdTime: string
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CommentGetResult>(
        'crm.deal.timeline.comment.get',
        {
          id: 77161,
          select: ['id', 'ownerTypeId', 'ownerId', 'text', 'authorId', 'createdTime'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Comment:', result.item.text)
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
      async function getComment() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.deal.timeline.comment.get',
            {
              id: 77161,
              select: ['id', 'ownerTypeId', 'ownerId', 'text', 'authorId', 'createdTime'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Comment:', result.item.text)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getComment)
    </script>
    ```

- Python

    CRM methods have no built-in wrappers in `b24pysdk`, and the SDK sends only methods from `Config().api_v3_methods` to REST 3.0. Add the method to this list and call it using `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.deal.timeline.comment.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 77161,
                "select": ["id", "ownerTypeId", "ownerId", "text", "authorId", "createdTime"],
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["item"])
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
                'crm.deal.timeline.comment.get',
                [
                    'id' => 77161,
                    'select' => ['id', 'ownerTypeId', 'ownerId', 'text', 'authorId', 'createdTime'],
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
        'crm.deal.timeline.comment.get',
        {
            id: 77161,
            select: ['id', 'ownerTypeId', 'ownerId', 'text', 'authorId', 'createdTime'],
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
        'crm.deal.timeline.comment.get',
        [
            'id' => 77161,
            'select' => ['id', 'ownerTypeId', 'ownerId', 'text', 'authorId', 'createdTime'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx have already been created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.deal.timeline.comment.get", b24.Params{
    	"id":     77161,
    	"select": []string{"id", "ownerTypeId", "ownerId", "text", "authorId", "createdTime"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.deal.timeline.comment.get: %w", err)
    }

    var result struct {
    	Item struct {
    		ID          int    `json:"id"`
    		OwnerTypeID int    `json:"ownerTypeId"`
    		OwnerID     int    `json:"ownerId"`
    		Text        string `json:"text"`
    		AuthorID    *int   `json:"authorId"`
    		CreatedTime string `json:"createdTime"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.Text)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 77161,
            "ownerTypeId": 2,
            "ownerId": 741,
            "text": "Sent a commercial proposal, awaiting a response by December 1",
            "authorId": 1,
            "createdTime": "2026-11-27T12:40:05+03:00"
        }
    },
    "time": {
        "start": 1795772416,
        "finish": 1795772417.01424,
        "duration": 1.014240026473999,
        "processing": 1,
        "date_start": "2026-11-27T12:40:16+03:00",
        "date_finish": "2026-11-27T12:40:17+03:00",
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
|| **result.item**
[`object`](../../../data-types.md) | Comment containing the fields from `select`, or only `id` if `select` is omitted. [Comment Fields](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Comment Fields {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Comment ID ||
|| **ownerTypeId**
[`integer`](../../../data-types.md) | ID of the object type whose item is linked to the comment, for example, `2` for a deal or `1038` for an SPA. For my company, the value is `4`, as for a company. The [crm.dictionary.entitytype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method returns the list of types ||
|| **ownerId**
[`integer`](../../../data-types.md) | ID of the CRM item linked to the comment ||
|| **text**
[`string`](../../../data-types.md) | Comment text. BBCode and HTML tags are returned in their stored form. Attached file tags, such as `[DISK FILE ID=n123]`, are removed from the text ||
|| **authorId**
[`integer`](../../../data-types.md) | Comment author ID. Retrieve the user’s data using [user.get](../../../user/user-get.md). If no author is recorded, the value is `null` ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Comment creation date and time ||
|#

The method does not return comment files. The previous version’s [crm.timeline.comment.get](../../timeline/comments/crm-timeline-comment-get.md) method returns the comment with attachments and the files in `FILES`.

If a comment is linked to multiple items of this type, `ownerId` contains the first item the user can view.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 77161 was not found."
    }
}
```

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Required field `id` is not specified | Pass the comment `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the comment ID as a positive number ||
|#

#### Comment Not Found {#entity-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Entity with ID `77161` was not found | The comment does not exist, is not linked to an item of the type specified in the method name, or you have no permission to view the item. The ID of another kind of timeline record, such as an activity, causes the same error. Check `id` and the method name: `crm.deal.timeline.comment.get` returns deal comments ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `files` found in entity `CommentDto` | Pass only fields from the [Comment Fields Table](#item) in `select` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Leads are disabled in the CRM settings. This error is returned only by the lead method. Enable leads ||
|#

#### Feature Unavailable on the Plan

Error Code: `FEATURE_NOT_AVAILABLE_ON_CURRENT_PLAN`, HTTP status **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | This feature is not available on your current plan. | The Bitrix24 plan does not include leads. This error is returned only by the lead method ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Method `crm.smartInvoice.timeline.comment.get` was not found | Check the method name: it must be lowercase, for example, `crm.smartinvoice.timeline.comment.get`. For an SPA, check `entityTypeId` using [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-timeline-comment-add-rest-v3.md)
- [{#T}](./crm-entity-timeline-comment-list-rest-v3.md)
- [{#T}](../../timeline/comments/crm-timeline-comment-get.md)
