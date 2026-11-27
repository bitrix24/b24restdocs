# Retrieve an Email Thread v 3.0 crm.activity.email.getthread

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the email

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

Retrieves the CRM email thread that includes the specified email. Each email in the thread contains the same fields and text as in the [crm.activity.email.get](./crm-activity-email-get-rest-v3.md) method. Replaces the removed `crm.activity.mail.getThread` method.

How the method builds the thread:

- emails are ordered by start time `startTime`, in ascending order
- the response includes up to 200 emails from the thread. If there are more emails, the method returns the first 200 and sets `isTruncated` to `true`
- the method silently skips emails you cannot view. Therefore, the response may contain fewer than 200 emails even when `isTruncated` is `true`
- if the email is not part of a thread, the method returns only that email

The method works only with emails: activities with `providerId` equal to `CRM_EMAIL`.

The method name is written in lowercase: `crm.activity.email.getthread`. The name `crm.activity.email.getThread` and other capitalization variants return the [Method Not Found](#method-not-found) error.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | ID of any email in the thread: an activity with `providerId` equal to `CRM_EMAIL`.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method using the `["providerId","=","CRM_EMAIL"]` condition ||
|#

The method does not accept `select`: emails always include all fields.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.activity.email.getthread`

{% endnote %}

The examples retrieve the thread that includes the email with ID `1831`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1831}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.email.getthread
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1831,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.activity.email.getthread
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EmailThreadResult = {
      isTruncated: boolean
      items: Array<{
        id: number
        subject: string
        direction: 'incoming' | 'outgoing'
        from: string | null
        to: string[] | null
        parentActivityId: number | null
        body: string | null
        startTime: string | null
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<EmailThreadResult>(
        'crm.activity.email.getthread',
        {
          id: 1831,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(letter => console.info(letter.startTime, letter.from, letter.subject))
        if (result.isTruncated) {
          console.info('Thread is longer than 200 letters')
        }
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
      async function getEmailThread() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.activity.email.getthread',
            {
              id: 1831,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(letter => console.info(letter.startTime, letter.from, letter.subject))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEmailThread)
    </script>
    ```

- Python

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.activity.email.getthread"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 1831,
            },
            prefer_version=3,
        )
        result = response["result"]
        for letter in result["items"]:
            print(letter["startTime"], letter["from"], letter["subject"])
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
                'crm.activity.email.getthread',
                [
                    'id' => 1831,
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
        'crm.activity.email.getthread',
        {
            id: 1831,
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
        'crm.activity.email.getthread',
        [
            'id' => 1831,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx have already been created — see the "SDK for Go" section
    res, err := client.Core().Call(ctx, "crm.activity.email.getthread", b24.Params{
    	"id": 1831,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.email.getthread: %w", err)
    }

    var result struct {
    	IsTruncated bool `json:"isTruncated"`
    	Items       []struct {
    		ID               int      `json:"id"`
    		Subject          string   `json:"subject"`
    		Direction        string   `json:"direction"`
    		From             *string  `json:"from"`
    		To               []string `json:"to"`
    		ParentActivityID *int     `json:"parentActivityId"`
    		Body             *string  `json:"body"`
    		StartTime        *string  `json:"startTime"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    for _, letter := range result.Items {
    	fmt.Println(letter.ID, letter.Direction, letter.Subject)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "isTruncated": false,
        "items": [
            {
                "subject": "Order No. 43061405",
                "direction": "incoming",
                "communications": [
                    {
                        "type": "EMAIL",
                        "value": "support@example.com",
                        "entityTypeId": 1,
                        "entityId": 257
                    }
                ],
                "from": "support@example.com",
                "to": [
                    "manager@example.com"
                ],
                "cc": [],
                "bcc": [],
                "parentActivityId": null,
                "body": "Hello! Please reply with photos of the product and its packaging.",
                "isBodyTruncated": false,
                "id": 1819,
                "providerId": "CRM_EMAIL",
                "isCompleted": true,
                "assignedById": 1,
                "authorId": 1,
                "editorId": 1,
                "createdTime": "2021-04-19T08:55:12+03:00",
                "updatedTime": "2021-04-19T08:55:12+03:00",
                "startTime": "2021-04-18T11:38:36+03:00",
                "endTime": "2021-04-19T18:00:00+03:00",
                "deadlineTime": "2021-04-19T18:00:00+03:00",
                "bindings": [
                    {
                        "entityTypeId": 3,
                        "entityId": 453
                    },
                    {
                        "entityTypeId": 4,
                        "entityId": 221
                    },
                    {
                        "entityTypeId": 2,
                        "entityId": 4143
                    }
                ]
            },
            {
                "subject": "Re: Order No. 43061405",
                "direction": "outgoing",
                "communications": [
                    {
                        "type": "EMAIL",
                        "value": "support@example.com",
                        "entityTypeId": 1,
                        "entityId": 257
                    }
                ],
                "from": "manager@example.com",
                "to": [
                    "support@example.com"
                ],
                "cc": [],
                "bcc": [],
                "parentActivityId": 1819,
                "body": "Hello! Thank you for your reply. Photos of the product and its packaging are attached.",
                "isBodyTruncated": false,
                "id": 1831,
                "providerId": "CRM_EMAIL",
                "isCompleted": true,
                "assignedById": 1,
                "authorId": 1,
                "editorId": 1,
                "createdTime": "2021-04-19T08:55:30+03:00",
                "updatedTime": "2021-04-19T08:55:30+03:00",
                "startTime": "2021-04-18T14:24:58+03:00",
                "endTime": "2021-04-19T18:00:00+03:00",
                "deadlineTime": "2021-04-19T18:00:00+03:00",
                "bindings": [
                    {
                        "entityTypeId": 3,
                        "entityId": 453
                    },
                    {
                        "entityTypeId": 4,
                        "entityId": 221
                    },
                    {
                        "entityTypeId": 2,
                        "entityId": 4143
                    }
                ]
            }
        ]
    },
    "time": {
        "start": 1795772340,
        "finish": 1795772341.188079,
        "duration": 1.1880791187286377,
        "processing": 0,
        "date_start": "2026-11-27T12:39:00+03:00",
        "date_finish": "2026-11-27T12:39:01+03:00",
        "operating_reset_at": 1795772940,
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
|| **result.isTruncated**
[`boolean`](../../../data-types.md) | `true` if the thread contains more than 200 emails and the method returned only the first 200 ||
|| **result.items**
[`object[]`](../../../data-types.md) | Emails in the thread that you can view, ordered by `startTime` in ascending order. Each email has the same fields as in the crm.activity.email.get response. [Email Field Descriptions](./crm-activity-email-get-rest-v3.md#fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID `803` was not found"
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
|| `id` | — | Pass a positive ID ||
|#

#### Email Not Found

Error code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | The email does not exist, the activity with this ID is not an email, you do not have permission to view the email, or its customer is hidden from you by access settings. Check `id` with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method using the `["providerId","=","CRM_EMAIL"]` condition ||
|#

#### Method Not Found {#method-not-found}

Error code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Check the method name: it is written in lowercase, `crm.activity.email.getthread`. The variant `crm.activity.email.getThread` does not work ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Exploring

- [{#T}](./crm-activity-email-get-rest-v3.md)
- [{#T}](./crm-activity-email-reply-rest-v3.md)
- [{#T}](./crm-entity-activity-email-list-rest-v3.md)
- [{#T}](./index.md)
