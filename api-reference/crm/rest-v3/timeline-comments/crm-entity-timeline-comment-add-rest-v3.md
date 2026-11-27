# Add a Timeline Comment v 3.0 crm.[type].timeline.comment.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM item; for my company, permission to edit my companies is required

{% note info "" %}

This method is part of REST 3.0. The calling conventions and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

`crm.<type>.timeline.comment.add` adds a text comment to a CRM item’s timeline. The `<type>` segment in the method name specifies the item type, and `ownerId` specifies the item. The user calling the method becomes the comment author.

## Method Name by Object Type

#|
|| **Object Type** | **Method** ||
|| Deal | `crm.deal.timeline.comment.add` ||
|| Lead | `crm.lead.timeline.comment.add` ||
|| Contact | `crm.contact.timeline.comment.add` ||
|| Company | `crm.company.timeline.comment.add` ||
|| My company | `crm.mycompany.timeline.comment.add` ||
|| Estimate | `crm.quote.timeline.comment.add` ||
|| Invoice | `crm.smartinvoice.timeline.comment.add` ||
|| CRM document | `crm.smartdocument.timeline.comment.add` ||
|| B2E document | `crm.smartb2edocument.timeline.comment.add` ||
|| SPA | `crm.smartprocess<entityTypeId>.timeline.comment.add`, for example, `crm.smartprocess1038.timeline.comment.add` ||
|#

For an SPA, substitute its type ID `entityTypeId` into the method name. The [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method returns SPA type IDs `entityTypeId`.

Company and my company are different types: `crm.company.timeline.comment.add` cannot find my company, and `crm.mycompany.timeline.comment.add` cannot find a regular company.

The method name must be lowercase: `crm.smartinvoice.timeline.comment.add`, rather than `crm.smartInvoice.timeline.comment.add`. A name with different casing returns a [Method Not Found](#method-not-found) error.

## Method Parameters

{% include [Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Comment fields. The required fields are `ownerId` and `text`. [Parameter Description](#fields) ||
|#

### fields Parameter {#fields}

{% include [Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerId***
[`integer`](../../../data-types.md) | ID of the CRM item to whose timeline the comment is added. A positive number, not a string.

Retrieve the ID using [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) ||
|| **text***
[`string`](../../../data-types.md) | Comment text. Leading and trailing spaces are removed. An empty string or a string containing only spaces causes an error.

The text is retained as is: the method does not process BBCode, such as `[b]payment[/b]`, or HTML tags, and returns them unchanged. Attachment tags `[DISK FILE ID=…]`, `[FILE ID=…]`, and `[DOCUMENT ID=…]` cause an error ||
|#

The method rejects other fields, such as `authorId` or `files`, with an error. REST 3.0 methods cannot attach files to comments. Use the previous version’s [crm.timeline.comment.add](../../timeline/comments/crm-timeline-comment-add.md) method with the `FILES` field.

{% note warning "" %}

Text length is limited to 65,535 bytes. The method truncates longer text without an error: it retains 65,535 of 70,000 Latin letters, or 32,767 of 70,000 Cyrillic letters.

{% endnote %}

## Code Examples

{% include [Examples](../../../../_includes/examples.md) %}

{% note info "" %}

Calls to the new API include the `/api/` segment in the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.deal.timeline.comment.add`

{% endnote %}

The examples add a comment to a deal’s timeline. For another object type, replace the method name using the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ownerId":741,"text":"Sent a commercial proposal, awaiting a response by December 1"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.timeline.comment.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ownerId":741,"text":"Sent a commercial proposal, awaiting a response by December 1"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.deal.timeline.comment.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods supported by `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method using the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CommentAddResult = {
      id: number
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CommentAddResult>(
        'crm.deal.timeline.comment.add',
        {
          fields: {
            ownerId: 741,
            text: 'Sent a commercial proposal, awaiting a response by December 1',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Comment ID:', result.id)
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
      async function addComment() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.deal.timeline.comment.add',
            {
              fields: {
                ownerId: 741,
                text: 'Sent a commercial proposal, awaiting a response by December 1',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Comment ID:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addComment)
    </script>
    ```

- Python

    CRM methods have no built-in wrappers in `b24pysdk`, and the SDK sends only methods from `Config().api_v3_methods` to REST 3.0. Add the method to this list and call it using `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.deal.timeline.comment.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "ownerId": 741,
                    "text": "Sent a commercial proposal, awaiting a response by December 1",
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["id"])
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
                'crm.deal.timeline.comment.add',
                [
                    'fields' => [
                        'ownerId' => 741,
                        'text' => 'Sent a commercial proposal, awaiting a response by December 1',
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
        'crm.deal.timeline.comment.add',
        {
            fields: {
                ownerId: 741,
                text: 'Sent a commercial proposal, awaiting a response by December 1',
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
        'crm.deal.timeline.comment.add',
        [
            'fields' => [
                'ownerId' => 741,
                'text' => 'Sent a commercial proposal, awaiting a response by December 1',
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
    res, err := client.Core().Call(ctx, "crm.deal.timeline.comment.add", b24.Params{
    	"fields": b24.Params{
    		"ownerId": 741,
    		"text":    "Sent a commercial proposal, awaiting a response by December 1",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.deal.timeline.comment.add: %w", err)
    }

    var created struct {
    	ID int `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &created); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(created.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 77161
    },
    "time": {
        "start": 1795772405,
        "finish": 1795772405.695934,
        "duration": 0.6959340572357178,
        "processing": 0,
        "date_start": "2026-11-27T12:40:05+03:00",
        "date_finish": "2026-11-27T12:40:05+03:00",
        "operating_reset_at": 1795772760,
        "operating": 0.9805641174316406
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Response data object ||
|| **result.id**
[`integer`](../../../data-types.md) | ID of the created comment. Retrieve the comment using [crm.&lt;type&gt;.timeline.comment.get](./crm-entity-timeline-comment-get-rest-v3.md) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

After a comment is added, Bitrix24 sends the [onCrmTimelineCommentAdd](../../timeline/comments/events/on-Crm-Timeline-Comment-Add.md) event.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Enter comment text.",
                "field": "fields.text"
            }
        ]
    }
}
```

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `fields` | Required field `fields` is not specified | Pass a `fields` object with `ownerId` and `text` ||
|| `fields.text` | Enter comment text | Pass non-empty text. A string containing only spaces is also considered empty ||
|| `fields.text` | Remove attachment tags from the comment text | Remove `[DISK FILE ID=…]`, `[FILE ID=…]`, and `[DOCUMENT ID=…]` tags from the text. Use the previous version’s [crm.timeline.comment.add](../../timeline/comments/crm-timeline-comment-add.md) method to attach a file ||
|| `fields.ownerId` | — | Pass the item ID as a positive number, not a string ||
|| `text` | — | Pass the text as a string ||
|| `fields.authorId` | — | Pass only `ownerId` and `text` in `fields`. The user calling the method becomes the author ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `ownerId` | — | Pass `ownerId` in `fields` ||
|| `text` | — | Pass `text` in `fields` ||
|#

#### Item Not Found {#entity-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | The item specified in `ownerId` does not exist for this object type, has been deleted, or you have no permission to edit it. Check `ownerId` and the method name: my company and a regular company are different types ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Pass only `ownerId` and `text` in `fields`. Use the previous version’s [crm.timeline.comment.add](../../timeline/comments/crm-timeline-comment-add.md) method to attach a file ||
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
|| `-` | — | Check the method name: it must be lowercase, for example, `crm.smartinvoice.timeline.comment.add`. For an SPA, check `entityTypeId` using [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-timeline-comment-get-rest-v3.md)
- [{#T}](./crm-entity-timeline-comment-list-rest-v3.md)
- [{#T}](../../timeline/comments/crm-timeline-comment-add.md)
