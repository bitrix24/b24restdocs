# Get the Email Content v 3.0 crm.activity.mail.getContent

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

The `crm.activity.mail.getContent` method returns a CRM email: the subject, sender, recipients, text, and the CRM items the email is bound to. The text is returned without HTML markup; long text is truncated.

The method works only with emails, which are activities with `providerId` equal to `CRM_EMAIL`. To find emails, use the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method with the `["providerId","=","CRM_EMAIL"]` condition.

The method name is written with a capital `C`: `crm.activity.mail.getContent`. If the name is written in lowercase, the method returns the "Method Not Found" error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **activityId***
[`integer`](../../../data-types.md) | Email ID, an activity with `providerId` equal to `CRM_EMAIL`.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.activity.mail.getContent`

{% endnote %}

The examples retrieve the email with the ID `747`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":747}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.mail.getContent
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":747,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.activity.mail.getContent
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type MailContentResult = {
      id: number
      subject: string
      from: string
      to: string[]
      body: string
      isBodyTruncated: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<MailContentResult>(
        'crm.activity.mail.getContent',
        {
          activityId: 747,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Email:', result.subject, result.body)
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
      async function getMailContent() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.activity.mail.getContent',
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
          console.info('Email:', result.subject, result.body)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMailContent)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.activity.mail.getContent"
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
                'crm.activity.mail.getContent',
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
        'crm.activity.mail.getContent',
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
        'crm.activity.mail.getContent',
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
    res, err := client.Core().Call(ctx, "crm.activity.mail.getContent", b24.Params{
    	"activityId": 747,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.mail.getContent: %w", err)
    }

    var result struct {
    	ID              int      `json:"id"`
    	Subject         string   `json:"subject"`
    	From            string   `json:"from"`
    	To              []string `json:"to"`
    	Body            string   `json:"body"`
    	IsBodyTruncated bool     `json:"isBodyTruncated"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.ID, result.Subject, result.From)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
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
        "body": "Good afternoon! Please find our commercial proposal attached.",
        "isBodyTruncated": false,
        "bindings": [
            {
                "entityTypeId": 2,
                "entityId": 4061
            },
            {
                "entityTypeId": 3,
                "entityId": 373
            }
        ]
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

The email data is returned directly in `result`, without the nested `item` key.

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Email data ||
|| **result.id**
[`integer`](../../../data-types.md) | Email ID ||
|| **result.subject**
[`string`](../../../data-types.md) | Email subject ||
|| **result.dateTime**
[`datetime`](../../../data-types.md) | Email date and time ||
|| **result.isIncoming**
[`boolean`](../../../data-types.md) | `true` for an incoming email, `false` for an outgoing one ||
|| **result.from**
[`string`](../../../data-types.md) | Sender address ||
|| **result.to**
[`string[]`](../../../data-types.md) | Recipient addresses ||
|| **result.cc**
[`string[]`](../../../data-types.md) | CC addresses ||
|| **result.bcc**
[`string[]`](../../../data-types.md) | BCC addresses ||
|| **result.body**
[`string`](../../../data-types.md) | Email text without HTML markup ||
|| **result.isBodyTruncated**
[`boolean`](../../../data-types.md) | `true` if the text is longer than 50,000 characters and has been truncated ||
|| **result.bindings**
[`object[]`](../../../data-types.md) | CRM items the email is bound to: `entityTypeId` is the object type, `entityId` is the item ID ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
|| `CRM_EMAIL_ACTIVITY_NOT_FOUND` | CRM email activity not found. | The email does not exist, or the activity with this ID is not an email. Check that the activity's `providerId` is `CRM_EMAIL`. HTTP status: **404** ||
|| `CRM_EMAIL_ACCESS_DENIED` | Access denied to the CRM email activity. | You have no permission to view the email: you have no access to the CRM items it is bound to. HTTP status: **403** ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-mail-get-thread-rest-v3.md)
- [{#T}](./crm-activity-mail-reply-rest-v3.md)
- [{#T}](./crm-entity-activity-list-rest-v3.md)
