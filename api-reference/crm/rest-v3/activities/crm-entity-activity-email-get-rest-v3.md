# Retrieve an Email v 3.0 crm.[type].activity.email.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the activity

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

Retrieves an email bound to a CRM item: its subject, direction, sender, recipients, text without markup, and the email it replies to.

The `<type>` segment in the method name specifies the object type: the method finds the email only if it is bound to at least one item of this type. It does not return other kinds of activities, such as calls and tasks. To retrieve them, use the [crm.&lt;type&gt;.activity.get](./crm-entity-activity-get-rest-v3.md) method.

To retrieve an email regardless of its object type, use the [crm.activity.email.get](./crm-activity-email-get-rest-v3.md) method.

## Method Name for the Object Type

#|
|| **Object Type** | **Method** ||
|| Deal | `crm.deal.activity.email.get` ||
|| Lead | `crm.lead.activity.email.get` ||
|| Contact | `crm.contact.activity.email.get` ||
|| Company | `crm.company.activity.email.get` ||
|| Estimate | `crm.quote.activity.email.get` ||
|| Invoice | `crm.smartinvoice.activity.email.get` ||
|| CRM Document | `crm.smartdocument.activity.email.get` ||
|| SPA | `crm.smartprocess<entityTypeId>.activity.email.get`, for example `crm.smartprocess1038.activity.email.get` ||
|#

For an SPA, substitute its type ID `entityTypeId` in the method name. SPA type IDs `entityTypeId` are returned by the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method.

The method name is written in lowercase: `crm.smartinvoice.activity.email.get`, rather than `crm.smartInvoice.activity.Email.get`. A name with different capitalization returns the [Method Not Found](#method-not-found) error.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Email ID: an activity with `providerId` = `CRM_EMAIL`.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.email.list](./crm-entity-activity-email-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of email fields to return, for example, `["id","subject","from","to","body"]`. Available fields are described in the [Item Object](#item) table. For `bindings` and `communications`, you can select nested fields using dot notation: `communications.value`.

Default: all email fields ||
|#

If you pass the `id` of another kind of activity, such as a call or task, the method returns a "not found" error.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.deal.activity.email.get`

{% endnote %}

The examples retrieve a deal's email with all fields. For another object type, replace the method name using the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8697}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.activity.email.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8697,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.deal.activity.email.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EmailGetResult = {
      item: {
        id: number
        subject: string
        direction: 'incoming' | 'outgoing' | 'undefined'
        from: string | null
        to: string[] | null
        cc: string[] | null
        bcc: string[] | null
        parentActivityId: number | null
        body: string | null
        isBodyTruncated: boolean | null
        isCompleted: boolean
        createdTime: string
        communications: Array<{ type: string; value: string | null; entityTypeId: number | null; entityId: number | null }>
        bindings: Array<{ entityTypeId: number; entityId: number }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<EmailGetResult>(
        'crm.deal.activity.email.get',
        {
          id: 8697,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Email:', result.item)
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
      async function getEmail() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.deal.activity.email.get',
            {
              id: 8697,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Email:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEmail)
    </script>
    ```

- Python

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.deal.activity.email.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 8697,
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
                'crm.deal.activity.email.get',
                [
                    'id' => 8697,
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
        'crm.deal.activity.email.get',
        {
            id: 8697,
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
        'crm.deal.activity.email.get',
        [
            'id' => 8697,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx have already been created — see the "SDK for Go" section
    res, err := client.Core().Call(ctx, "crm.deal.activity.email.get", b24.Params{
    	"id": 8697,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.deal.activity.email.get: %w", err)
    }

    var result struct {
    	Item struct {
    		ID               int      `json:"id"`
    		Subject          string   `json:"subject"`
    		Direction        string   `json:"direction"`
    		From             *string  `json:"from"`
    		To               []string `json:"to"`
    		Cc               []string `json:"cc"`
    		Bcc              []string `json:"bcc"`
    		ParentActivityID *int     `json:"parentActivityId"`
    		Body             *string  `json:"body"`
    		IsBodyTruncated  *bool    `json:"isBodyTruncated"`
    		CreatedTime      string   `json:"createdTime"`
    		Bindings         []struct {
    			EntityTypeID int `json:"entityTypeId"`
    			EntityID     int `json:"entityId"`
    		} `json:"bindings"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.Subject, result.Item.Direction)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "subject": "Re: Sales Proposal",
            "direction": "outgoing",
            "communications": [
                {
                    "type": "EMAIL",
                    "value": "client@example.com",
                    "entityTypeId": 2,
                    "entityId": 741
                }
            ],
            "from": "sales@example.com",
            "to": [
                "client@example.com"
            ],
            "cc": [],
            "bcc": [],
            "parentActivityId": 8691,
            "body": "Thank you for your email! We will reply by the end of the day.\n\nSent from Bitrix24",
            "isBodyTruncated": false,
            "id": 8697,
            "providerId": "CRM_EMAIL",
            "isCompleted": true,
            "assignedById": 1,
            "authorId": 1,
            "editorId": 1,
            "createdTime": "2026-11-27T12:41:56+03:00",
            "updatedTime": "2026-11-27T12:41:58+03:00",
            "startTime": "2026-11-27T12:41:56+03:00",
            "endTime": "2026-11-27T12:41:56+03:00",
            "deadlineTime": "2026-11-27T12:41:56+03:00",
            "bindings": [
                {
                    "entityTypeId": 2,
                    "entityId": 741
                },
                {
                    "entityTypeId": 3,
                    "entityId": 71
                }
            ]
        }
    },
    "time": {
        "start": 1795772613,
        "finish": 1795772613.318065,
        "duration": 0.31806492805480957,
        "processing": 0,
        "date_start": "2026-11-27T12:43:33+03:00",
        "date_finish": "2026-11-27T12:43:33+03:00",
        "operating_reset_at": 1795772880,
        "operating": 1.7879889011383057
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
[`object`](../../../data-types.md) | Email with the fields specified in `select`, or all fields if `select` is omitted. [Field Descriptions](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

#### Item Object {#item}

The fields are read-only. The Filter and Sorting columns apply to the [crm.&lt;type&gt;.activity.email.list](./crm-entity-activity-email-list-rest-v3.md) method.

#|
|| **Name**
`type` | **Description** | **Filter** | **Sorting** ||
|| **id**
[`integer`](../../../data-types.md) | Email ID | + | + ||
|| **providerId**
[`string`](../../../data-types.md) | Activity provider, always `CRM_EMAIL` for emails | + | — ||
|| **isCompleted**
[`boolean`](../../../data-types.md) | Whether the email is completed as an activity. Possible values:

- `true` — completed
- `false` — not completed | + | — ||
|| **assignedById**
[`integer`](../../../data-types.md) | Responsible person's ID | + | — ||
|| **authorId**
[`integer`](../../../data-types.md) | Author ID | — | — ||
|| **editorId**
[`integer`](../../../data-types.md) | ID of the user who last modified the activity | — | — ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Creation date and time | + | + ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Last modification date and time | + | + ||
|| **startTime**
[`datetime`](../../../data-types.md) | Activity start date and time. For an outgoing email, the time it was sent | + | + ||
|| **endTime**
[`datetime`](../../../data-types.md) | Activity end date and time | — | + ||
|| **deadlineTime**
[`datetime`](../../../data-types.md) | Activity deadline, `null` if there is no deadline | + | + ||
|| **bindings**
[`object[]`](../../../data-types.md) | CRM items the email is bound to. [Object Description](#bindings) | — | — ||
|| **subject**
[`string`](../../../data-types.md) | Email subject | — | — ||
|| **direction**
[`string`](../../../data-types.md) | Email direction. Possible values:

- `incoming` — incoming
- `outgoing` — outgoing
- `undefined` — direction is not specified | + | — ||
|| **communications**
[`object[]`](../../../data-types.md) | Addresses associated with the email and the CRM items that own these addresses. [Object Description](#communications) | — | — ||
|| **from**
[`string`](../../../data-types.md) | Sender as specified in the email: an address or `Name <address>` | — | — ||
|| **to**
[`string[]`](../../../data-types.md) | Recipient addresses | — | — ||
|| **cc**
[`string[]`](../../../data-types.md) | CC addresses. An empty array is returned if there are none | — | — ||
|| **bcc**
[`string[]`](../../../data-types.md) | BCC addresses. An empty array is returned if there are none | — | — ||
|| **parentActivityId**
[`integer`](../../../data-types.md) | ID of the email this email replies to, `null` if it is not a reply | — | — ||
|| **body**
[`string`](../../../data-types.md) | Email text without HTML markup. Line breaks are preserved; text longer than 50,000 characters is truncated. The signature added by Bitrix24 when sending is included in the text | — | — ||
|| **isBodyTruncated**
[`boolean`](../../../data-types.md) | Whether the email text in `body` is truncated. Possible values:

- `true` — the text is longer than 50,000 characters and is truncated
- `false` — the full text is returned | — | — ||
|#

The method does not return attachments or HTML email text. The `type` and `description` fields and activity files are returned by the [crm.&lt;type&gt;.activity.get](./crm-entity-activity-get-rest-v3.md) method.

#### Bindings Object {#bindings}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | CRM object type ID, for example, `2` — deal, `3` — contact ||
|| **entityId**
[`integer`](../../../data-types.md) | CRM item ID ||
|#

#### Communications Object {#communications}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Communication channel type, `EMAIL` for emails ||
|| **value**
[`string`](../../../data-types.md) | Email address. Returns `null` if the address is not associated with a CRM item or you cannot access that item ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | ID of the CRM object type that owns the address, for example, `3` — contact. Returns `null` if the address is not associated with a CRM item or you cannot access that item ||
|| **entityId**
[`integer`](../../../data-types.md) | ID of the CRM item that owns the address. Returns `null` if the address is not associated with a CRM item or you cannot access that item ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID `7773` was not found"
    }
}
```

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Required field `id` is not specified | Pass the email `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|| `id` | Field value must be 1 or greater | Pass a positive ID ||
|#

#### Email Not Found

Error code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Entity with ID `7773` was not found | The activity does not exist, is not an email, is not bound to an item of this object type, you do not have permission to view it, or its customer is hidden from you by access settings. Check `id` with the [crm.&lt;type&gt;.activity.email.list](./crm-entity-activity-email-list-rest-v3.md) method and check the method name ||
|#

#### Unknown Field Error

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `description` found in entity `EmailActivityDto` | Check the field names in `select` against the [Item Object](#item) table. The `type` and `description` fields and activity files are returned by the [crm.&lt;type&gt;.activity.get](./crm-entity-activity-get-rest-v3.md) method ||
|| `-` | Unknown field `foo` found in entity `ActivityCommunicationDto` | Check the nested field in `select`: `communications.type`, `communications.value`, `communications.entityTypeId`, `communications.entityId` ||
|#

#### Access Error

Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | For `crm.lead.activity.email.get`: leads are disabled in Bitrix24. Enable leads or retrieve the email through another object type it is also bound to ||
|#

#### Method Not Found {#method-not-found}

Error code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Method `crm.deal.activity.Email.get` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.activity.email.get`. For an SPA, check `entityTypeId` with the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method. My company and B2E documents have no email methods ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](./crm-entity-activity-email-list-rest-v3.md)
- [{#T}](./crm-entity-activity-email-send-rest-v3.md)
- [{#T}](./crm-activity-email-get-rest-v3.md)
- [{#T}](./crm-activity-email-reply-rest-v3.md)
