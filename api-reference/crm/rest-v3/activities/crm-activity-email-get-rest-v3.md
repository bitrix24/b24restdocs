# Retrieve an Email v 3.0 crm.activity.email.get

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

Retrieves a CRM email by ID: its subject, direction, sender, recipients, text, and the CRM items it is bound to. You do not need to specify the object type the email is bound to. Replaces the removed `crm.activity.mail.getContent` method.

The method works only with emails: activities with `providerId` equal to `CRM_EMAIL`. To find emails, use the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method with the `["providerId","=","CRM_EMAIL"]` condition.

The email text is returned without HTML markup. If the text is longer than 50,000 characters, the method truncates it and sets `isBodyTruncated` to `true`.

The method name is written in lowercase: `crm.activity.email.get`. A name with different capitalization returns the [Method Not Found](#method-not-found) error.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Email ID: an activity with `providerId` equal to `CRM_EMAIL`.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of email fields to return, for example, `["id","subject","from","body"]`. Available fields are described in [Returned Data](#fields). For `bindings` and `communications`, you can select nested fields using dot notation: `bindings.entityId`.

Default: all email fields ||
|#

The `select` parameter accepts the `assignedBy`, `author`, and `editor` fields with employee data, but the response returns only the ID: `assignedById`, `authorId`, or `editorId`.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.activity.email.get`

{% endnote %}

The examples retrieve the email with ID `1831`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1831}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.email.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1831,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.activity.email.get
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
        direction: 'incoming' | 'outgoing'
        from: string | null
        to: string[] | null
        cc: string[] | null
        bcc: string[] | null
        parentActivityId: number | null
        body: string | null
        isBodyTruncated: boolean | null
        bindings: Array<{ entityTypeId: number; entityId: number }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<EmailGetResult>(
        'crm.activity.email.get',
        {
          id: 1831,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Email:', result.item.subject, result.item.body)
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
            'crm.activity.email.get',
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
          console.info('Email:', result.item.subject, result.item.body)
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

    method = "crm.activity.email.get"
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.email.get',
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
        'crm.activity.email.get',
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
        'crm.activity.email.get',
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
    res, err := client.Core().Call(ctx, "crm.activity.email.get", b24.Params{
    	"id": 1831,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.email.get: %w", err)
    }

    var result struct {
    	Item struct {
    		ID               int      `json:"id"`
    		Subject          string   `json:"subject"`
    		Direction        string   `json:"direction"`
    		From             *string  `json:"from"`
    		To               []string `json:"to"`
    		ParentActivityID *int     `json:"parentActivityId"`
    		Body             *string  `json:"body"`
    		IsBodyTruncated  *bool    `json:"isBodyTruncated"`
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
    },
    "time": {
        "start": 1795772596,
        "finish": 1795772597.504591,
        "duration": 1.5045909881591797,
        "processing": 1,
        "date_start": "2026-11-27T12:43:16+03:00",
        "date_finish": "2026-11-27T12:43:17+03:00",
        "operating_reset_at": 1795773180,
        "operating": 0.9565129280090332
    }
}
```

### Returned Data {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Response data object ||
|| **result.item**
[`object`](../../../data-types.md) | Email with the fields specified in `select`, or all fields if `select` is omitted ||
|| **result.item.id**
[`integer`](../../../data-types.md) | Email ID ||
|| **result.item.providerId**
[`string`](../../../data-types.md) | Activity provider, always `CRM_EMAIL` for an email ||
|| **result.item.subject**
[`string`](../../../data-types.md) | Email subject ||
|| **result.item.direction**
[`string`](../../../data-types.md) | Email direction: `incoming` — incoming, `outgoing` — outgoing ||
|| **result.item.from**
[`string`](../../../data-types.md) | Sender address. `null` if the email has no address data, for example, an email created without sending ||
|| **result.item.to**
[`string[]`](../../../data-types.md) | Recipient addresses ||
|| **result.item.cc**
[`string[]`](../../../data-types.md) | CC addresses ||
|| **result.item.bcc**
[`string[]`](../../../data-types.md) | BCC addresses ||
|| **result.item.parentActivityId**
[`integer`](../../../data-types.md) | ID of the email this email replies to. `null` if the email is not a reply ||
|| **result.item.body**
[`string`](../../../data-types.md) | Email text without HTML markup, up to 50,000 characters ||
|| **result.item.isBodyTruncated**
[`boolean`](../../../data-types.md) | `true` if the text is longer than 50,000 characters and is truncated ||
|| **result.item.communications**
[`object[]`](../../../data-types.md) | Communication contacts: `type` is the channel, `EMAIL` for emails; `value` is the address; `entityTypeId` and `entityId` identify the CRM item that owns the address. If you cannot access the item, `value`, `entityTypeId`, and `entityId` are `null` ||
|| **result.item.isCompleted**
[`boolean`](../../../data-types.md) | `true` — the email is completed as an activity, `false` — in progress ||
|| **result.item.assignedById**
[`integer`](../../../data-types.md) | Responsible person's ID ||
|| **result.item.authorId**
[`integer`](../../../data-types.md) | Author ID ||
|| **result.item.editorId**
[`integer`](../../../data-types.md) | ID of the employee who last modified the email ||
|| **result.item.createdTime**
[`datetime`](../../../data-types.md) | Creation date and time ||
|| **result.item.updatedTime**
[`datetime`](../../../data-types.md) | Last modification date and time ||
|| **result.item.startTime**
[`datetime`](../../../data-types.md) | Activity start time; for a sent email, the time it was sent. Can be `null` ||
|| **result.item.endTime**
[`datetime`](../../../data-types.md) | Activity end time. Can be `null` ||
|| **result.item.deadlineTime**
[`datetime`](../../../data-types.md) | Deadline. `null` if there is no deadline ||
|| **result.item.bindings**
[`object[]`](../../../data-types.md) | CRM items the email is bound to: `entityTypeId` is the object type; `entityId` is the item ID ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

Empty `to`, `cc`, `bcc`, `bindings`, and `communications` are returned as empty arrays `[]`. If the email's customer is hidden from you by access settings, the method returns an email not found error.

The method does not return activity fields unrelated to email, such as `description`, `files`, and `type`. They are returned by the [crm.&lt;type&gt;.activity.get](./crm-entity-activity-get-rest-v3.md) method.

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

#### Unknown Field Error

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Check the field names in `select` against the [Email Field Table](#fields) ||
|| `-` | — | Check the nested `bindings` field in `select`: `entityTypeId` and `entityId` are available ||
|| `-` | — | Check the employee field name in `select`, for example, `assignedBy.firstName` ||
|#

#### Method Not Found {#method-not-found}

Error code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Check the method name: it is written in lowercase, `crm.activity.email.get` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Exploring

- [{#T}](./crm-activity-email-get-thread-rest-v3.md)
- [{#T}](./crm-activity-email-reply-rest-v3.md)
- [{#T}](./crm-entity-activity-email-list-rest-v3.md)
- [{#T}](./crm-entity-activity-get-rest-v3.md)
- [{#T}](./index.md)
