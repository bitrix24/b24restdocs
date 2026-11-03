# Get the Email Thread v 3.0 crm.activity.mail.getThread

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the email

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.activity.mail.getThread` method returns the CRM email thread that includes the specified email: all emails of the conversation in ascending order of time, up to 200 emails. The response does not contain the email text; it is returned by the [crm.activity.mail.getContent](./crm-activity-mail-get-content-rest-v3.md) method.

Thread emails you cannot view are returned with `isHidden: true` and without email data.

The method name is written with a capital `T`: `crm.activity.mail.getThread`. If the name is written in lowercase, the method returns the "Method Not Found" error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **activityId***
[`integer`](../../../data-types.md) | ID of any email in the thread, an activity with `providerId` equal to `CRM_EMAIL`.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.activity.mail.getThread`

{% endnote %}

The examples retrieve the thread of the email with the ID `747`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":747}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.mail.getThread
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":747,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.activity.mail.getThread
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type MailThreadResult = {
      messages: Array<{
        id: number
        subject?: string
        from?: string
        isHidden: boolean
      }>
      isTruncated: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<MailThreadResult>(
        'crm.activity.mail.getThread',
        {
          activityId: 747,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Thread:', result.messages)
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
      async function getMailThread() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.activity.mail.getThread',
            {
              activityId: 747,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Thread:', result.messages)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMailThread)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.activity.mail.getThread"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "activityId": 747,
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
                'crm.activity.mail.getThread',
                [
                    'activityId' => 747,
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
        'crm.activity.mail.getThread',
        {
            activityId: 747,
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
        'crm.activity.mail.getThread',
        [
            'activityId' => 747,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.mail.getThread", b24.Params{
    	"activityId": 747,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.mail.getThread: %w", err)
    }

    var result struct {
    	Messages []struct {
    		ID       int    `json:"id"`
    		Subject  string `json:"subject"`
    		From     string `json:"from"`
    		IsHidden bool   `json:"isHidden"`
    	} `json:"messages"`
    	IsTruncated bool `json:"isTruncated"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, message := range result.Messages {
    	fmt.Println(message.ID, message.Subject, message.IsHidden)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "messages": [
            {
                "id": 747,
                "subject": "Commercial proposal",
                "dateTime": "2026-10-28T12:19:18+03:00",
                "isIncoming": true,
                "from": "client@example.com",
                "to": [
                    "sales@example.com"
                ],
                "cc": [],
                "bcc": [],
                "isHidden": false
            }
        ],
        "isTruncated": false
    },
    "time": {
        "start": 1793711360,
        "finish": 1793711360.764732,
        "duration": 0.7647318840026855,
        "processing": 0,
        "date_start": "2026-11-03T16:09:20+03:00",
        "date_finish": "2026-11-03T16:09:20+03:00",
        "operating_reset_at": 1793711940,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Thread data ||
|| **result.messages**
[`object[]`](../../../data-types.md) | Thread emails in ascending order of time. [Email description](#message) ||
|| **result.isTruncated**
[`boolean`](../../../data-types.md) | `true` if the thread contains more than 200 emails and the response includes the first 200 ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Thread Email {#message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Email ID. For a hidden email, `0` ||
|| **subject**
[`string`](../../../data-types.md) | Email subject. Absent for a hidden email ||
|| **dateTime**
[`datetime`](../../../data-types.md) | Email date and time. Absent for a hidden email ||
|| **isIncoming**
[`boolean`](../../../data-types.md) | `true` for an incoming email, `false` for an outgoing one. Absent for a hidden email ||
|| **from**
[`string`](../../../data-types.md) | Sender address. Absent for a hidden email ||
|| **to**
[`string[]`](../../../data-types.md) | Recipient addresses. Absent for a hidden email ||
|| **cc**
[`string[]`](../../../data-types.md) | CC addresses. For a hidden email, `null` ||
|| **bcc**
[`string[]`](../../../data-types.md) | BCC addresses. For a hidden email, `null` ||
|| **isHidden**
[`boolean`](../../../data-types.md) | `true` if the email is not available to you and has no email data ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "CRM_EMAIL_ACTIVITY_NOT_FOUND",
        "message": "CRM email activity not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Errors

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `CRM_EMAIL_INVALID_REQUEST` | activityId must be a positive integer. | Pass `activityId` as a positive integer. HTTP status: **400** ||
|| `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION` | This request requires data type `int` in field `activityId` | Pass `activityId` as a number, not a string. HTTP status: **400** ||
|#

#### Email Not Found

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `CRM_EMAIL_ACTIVITY_NOT_FOUND` | CRM email activity not found. | The email does not exist, or the activity with this ID is not an email. HTTP status: **404** ||
|| `CRM_EMAIL_ACCESS_DENIED` | Access denied to the CRM email activity. | You have no permission to view the email: you have no access to the CRM items it is bound to. HTTP status: **403** ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-mail-get-content-rest-v3.md)
- [{#T}](./crm-activity-mail-reply-rest-v3.md)
