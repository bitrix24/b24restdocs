# Get a Call v 3.0 crm.[type].activity.call.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the activity

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.activity.call.get` method returns a telephony call bound to a CRM item: direction, duration, outcome, call recording, comment, client rating, line, and number.

The object type is set by the `<type>` segment of the method name: the method finds the call only if it is bound to at least one item of this type. The method does not return activities of other kinds, such as meetings and tasks. These activities and the general call fields, such as `subject`, are returned by the [crm.&lt;type&gt;.activity.get](./crm-entity-activity-get-rest-v3.md) method.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.activity.call.get` ||
|| Lead | `crm.lead.activity.call.get` ||
|| Contact | `crm.contact.activity.call.get` ||
|| Company | `crm.company.activity.call.get` ||
|| Estimate | `crm.quote.activity.call.get` ||
|| Invoice | `crm.smartinvoice.activity.call.get` ||
|| CRM document | `crm.smartdocument.activity.call.get` ||
|| SPA | `crm.smartprocess<entityTypeId>.activity.call.get`, for example, `crm.smartprocess1038.activity.call.get` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs `entityTypeId` are returned by the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method.

The method name is written in lowercase: `crm.smartinvoice.activity.call.get`, not `crm.smartInvoice.activity.Call.get`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Call ID — the ID of an activity with `providerId` = `VOXIMPLANT_CALL`.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.call.list](./crm-entity-activity-call-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of call fields to return, for example, `["id","direction","duration","outcome"]`. The available fields are described in the [Item Object](#item) table. For `bindings`, `communications`, and `record`, you can select nested fields using dot notation: `record.downloadUrl`.

Default: all call fields ||
|#

For the `id` of an activity of another kind, such as a meeting or an Open Channel chat, the method returns a "not found" error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.activity.call.get`

{% endnote %}

The examples retrieve a call of an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8471,"select":["id","direction","duration","outcome","record","comment","clientVote","portalLine","phoneNumber","assignedById","startTime","bindings"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.activity.call.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8471,"select":["id","direction","duration","outcome","record","comment","clientVote","portalLine","phoneNumber","assignedById","startTime","bindings"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.activity.call.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CallGetResult = {
      item: {
        id: number
        direction: 'incoming' | 'outgoing' | 'undefined'
        duration: number | null
        outcome: string | null
        record: { id: string; name: string; url: string; downloadUrl: string } | null
        comment: string | null
        clientVote: number | null
        portalLine: string | null
        phoneNumber: string | null
        assignedById: number | null
        startTime: string | null
        bindings: Array<{ entityTypeId: number; entityId: number }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CallGetResult>(
        'crm.smartprocess1038.activity.call.get',
        {
          id: 8471,
          select: ['id', 'direction', 'duration', 'outcome', 'record', 'comment', 'clientVote', 'portalLine', 'phoneNumber', 'assignedById', 'startTime', 'bindings'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Call:', result.item)
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
      async function getCall() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.activity.call.get',
            {
              id: 8471,
              select: ['id', 'direction', 'duration', 'outcome', 'record', 'comment', 'clientVote', 'portalLine', 'phoneNumber', 'assignedById', 'startTime', 'bindings'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Call:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCall)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.activity.call.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 8471,
                "select": ["id", "direction", "duration", "outcome", "record", "comment", "clientVote", "portalLine", "phoneNumber", "assignedById", "startTime", "bindings"],
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
                'crm.smartprocess1038.activity.call.get',
                [
                    'id' => 8471,
                    'select' => ['id', 'direction', 'duration', 'outcome', 'record', 'comment', 'clientVote', 'portalLine', 'phoneNumber', 'assignedById', 'startTime', 'bindings'],
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
        'crm.smartprocess1038.activity.call.get',
        {
            id: 8471,
            select: ['id', 'direction', 'duration', 'outcome', 'record', 'comment', 'clientVote', 'portalLine', 'phoneNumber', 'assignedById', 'startTime', 'bindings'],
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
        'crm.smartprocess1038.activity.call.get',
        [
            'id' => 8471,
            'select' => ['id', 'direction', 'duration', 'outcome', 'record', 'comment', 'clientVote', 'portalLine', 'phoneNumber', 'assignedById', 'startTime', 'bindings'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.activity.call.get", b24.Params{
    	"id":     8471,
    	"select": []string{"id", "direction", "duration", "outcome", "record", "comment", "clientVote", "portalLine", "phoneNumber", "assignedById", "startTime", "bindings"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.activity.call.get: %w", err)
    }

    var result struct {
    	Item struct {
    		ID        int     `json:"id"`
    		Direction string  `json:"direction"`
    		Duration  *int    `json:"duration"`
    		Outcome   *string `json:"outcome"`
    		Record    *struct {
    			ID          string `json:"id"`
    			Name        string `json:"name"`
    			DownloadURL string `json:"downloadUrl"`
    		} `json:"record"`
    		Comment      *string `json:"comment"`
    		ClientVote   *int    `json:"clientVote"`
    		PortalLine   *string `json:"portalLine"`
    		PhoneNumber  *string `json:"phoneNumber"`
    		AssignedByID *int    `json:"assignedById"`
    		StartTime    *string `json:"startTime"`
    		Bindings     []struct {
    			EntityTypeID int `json:"entityTypeId"`
    			EntityID     int `json:"entityId"`
    		} `json:"bindings"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.Direction, result.Item.Outcome)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "direction": "incoming",
            "duration": 95,
            "outcome": "successful",
            "record": {
                "id": "9079",
                "name": "call-001.mp3",
                "url": "/bitrix/tools/crm_show_file.php?fileId=9079&ownerTypeId=6&ownerId=8471",
                "downloadUrl": "/bitrix/tools/crm_show_file.php?fileId=9079&ownerTypeId=6&ownerId=8471"
            },
            "comment": "The client asks to call back after lunch",
            "clientVote": 5,
            "portalLine": "reg100001",
            "phoneNumber": "+49300000001",
            "id": 8471,
            "assignedById": 1269,
            "startTime": "2026-11-10T11:19:38+01:00",
            "bindings": [
                {
                    "entityTypeId": 1038,
                    "entityId": 41
                },
                {
                    "entityTypeId": 3,
                    "entityId": 797
                }
            ]
        }
    },
    "time": {
        "start": 1795430968,
        "finish": 1795430968.363972,
        "duration": 0.3639719486236572,
        "processing": 0,
        "date_start": "2026-11-23T13:49:28+01:00",
        "date_finish": "2026-11-23T13:49:28+01:00",
        "operating_reset_at": 1795431540,
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
|| **result.item**
[`object`](../../../data-types.md) | Call with the fields from `select`; without `select`, with all fields. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

All fields are read-only: calls are created and changed by telephony. The "Filter" and "Sorting" columns apply to the [crm.&lt;type&gt;.activity.call.list](./crm-entity-activity-call-list-rest-v3.md) method.

#|
|| **Name**
`type` | **Description** | **Filter** | **Sorting** ||
|| **id**
[`integer`](../../../data-types.md) | Call ID | + | + ||
|| **providerId**
[`string`](../../../data-types.md) | Activity provider, always `VOXIMPLANT_CALL` for calls | + | — ||
|| **isCompleted**
[`boolean`](../../../data-types.md) | Indicates that the call is completed as an activity | + | — ||
|| **assignedById**
[`integer`](../../../data-types.md) | ID of the responsible person | + | — ||
|| **authorId**
[`integer`](../../../data-types.md) | Author ID | — | — ||
|| **editorId**
[`integer`](../../../data-types.md) | ID of the user who last changed the activity | — | — ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Creation date and time | + | + ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Date and time of the last change | + | + ||
|| **startTime**
[`datetime`](../../../data-types.md) | Call start date and time | + | + ||
|| **endTime**
[`datetime`](../../../data-types.md) | Call end date and time | — | + ||
|| **deadlineTime**
[`datetime`](../../../data-types.md) | Activity deadline, `null` if there is no deadline | + | + ||
|| **bindings**
[`object[]`](../../../data-types.md) | CRM items the call is bound to. [Object description](#bindings) | — | — ||
|| **direction**
[`string`](../../../data-types.md) | Call direction. Possible values:

- `incoming` — incoming
- `outgoing` — outgoing
- `undefined` — no direction | + | — ||
|| **communications**
[`object[]`](../../../data-types.md) | Call participants. [Object description](#communications) | — | — ||
|| **callId**
[`string`](../../../data-types.md) | Call ID in telephony, `null` if the call was not created by telephony | — | — ||
|| **isCallback**
[`boolean`](../../../data-types.md) | Indicates a callback requested by the client. The field is not returned if the call has no telephony data | — | — ||
|| **duration**
[`integer`](../../../data-types.md) | Conversation duration in seconds | — | — ||
|| **outcome**
[`string`](../../../data-types.md) | Call outcome, `null` if there is no outcome yet. Possible values:

- `successful` — successful call
- `missed` — missed
- `declined` — declined
- `canceled` — canceled
- `busy` — busy
- `unavailable` — subscriber unavailable
- `invalidNumber` — invalid number
- `insufficientFunds` — insufficient funds
- `unavailableOnPlan` — not available on the plan
- `forbidden` — forbidden
- `blocked` — blocked
- `routeUnavailable` — route unavailable
- `serverError` — server error
- `other` — other outcome | — | — ||
|| **record**
[`object`](../../../data-types.md) | Call recording — the first audio file of the call, `null` if there is no recording. [Object description](#record) | — | — ||
|| **comment**
[`string`](../../../data-types.md) | Call comment in telephony | — | — ||
|| **clientVote**
[`integer`](../../../data-types.md) | Client rating from `1` to `5`, `null` if the client has not rated the call | — | — ||
|| **portalLine**
[`string`](../../../data-types.md) | Bitrix24 line the call went through | — | — ||
|| **phoneNumber**
[`string`](../../../data-types.md) | Phone number of the other party | — | — ||
|#

The `callId`, `duration`, `outcome`, `comment`, `clientVote`, `portalLine`, and `phoneNumber` fields are taken from the telephony statistics. If the call was not created by telephony, for example, manually, these fields are `null`. If you have no access to the call client, `record`, `comment`, `clientVote`, and `phoneNumber` are also `null`.

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
[`string`](../../../data-types.md) | Communication channel type, `PHONE` for calls ||
|| **value**
[`string`](../../../data-types.md) | Phone number. If you have no access to the address, `null` is returned ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | CRM object type ID of the participant ||
|| **entityId**
[`integer`](../../../data-types.md) | CRM item ID of the participant ||
|#

#### Record Object {#record}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Recording file ID ||
|| **name**
[`string`](../../../data-types.md) | File name, for example, `call-001.mp3` ||
|| **url**
[`string`](../../../data-types.md) | Relative link to view the file ||
|| **downloadUrl**
[`string`](../../../data-types.md) | Relative link to download the file ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 7593 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the call `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|| `id` | Field value must be 1 or greater | Pass a positive ID ||
|#

#### Call Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `7593` was not found | The activity does not exist, is not a call, the call is not bound to an item of this object type, or you have no permission to view it. Check `id` with the [crm.&lt;type&gt;.activity.call.list](./crm-entity-activity-call-list-rest-v3.md) method and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `subject` found in entity `CallActivityDto` | Check the field names in `select` against the [Item Object](#item) table. The `subject`, `description`, `type` fields and activity files are returned by the [crm.&lt;type&gt;.activity.get](./crm-entity-activity-get-rest-v3.md) method ||
|| `-` | Unknown field `foo` found in entity `FileDto` | Check the nested recording field in `select`: `record.id`, `record.name`, `record.url`, `record.downloadUrl` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | For `crm.lead.activity.call.get`: leads are disabled on Bitrix24. Enable leads or read the call through another object type it is also bound to ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.deal.activity.Call.get` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.activity.call.get`. For an SPA, check `entityTypeId` with the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method. My company and B2E documents have no call methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-activity-call-list-rest-v3.md)
- [{#T}](./crm-entity-activity-get-rest-v3.md)
- [{#T}](./crm-entity-activity-openline-get-rest-v3.md)
