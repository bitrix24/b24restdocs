# Get an Open Channel Chat v 3.0 crm.[type].activity.openline.get

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

The `crm.<type>.activity.openline.get` method returns an Open Channel chat bound to a CRM item: direction, Open Channel, channel, session, chat, and client rating.

The object type is set by the `<type>` segment of the method name: the method finds the chat only if it is bound to at least one item of this type. The method does not return activities of other kinds, such as calls and meetings. These activities and the general activity fields, such as `subject`, are returned by the [crm.&lt;type&gt;.activity.get](./crm-entity-activity-get-rest-v3.md) method.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.activity.openline.get` ||
|| Lead | `crm.lead.activity.openline.get` ||
|| Contact | `crm.contact.activity.openline.get` ||
|| Company | `crm.company.activity.openline.get` ||
|| Estimate | `crm.quote.activity.openline.get` ||
|| Invoice | `crm.smartinvoice.activity.openline.get` ||
|| CRM document | `crm.smartdocument.activity.openline.get` ||
|| SPA | `crm.smartprocess<entityTypeId>.activity.openline.get`, for example, `crm.smartprocess1038.activity.openline.get` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs `entityTypeId` are returned by the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method.

The method name is written in lowercase: `crm.deal.activity.openline.get`, not `crm.deal.activity.openLine.get`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Chat ID — the ID of an activity with `providerId` = `IMOPENLINES_SESSION`.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.openline.list](./crm-entity-activity-openline-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of chat fields to return, for example, `["id","connectorId","sessionId","chatId"]`. The available fields are described in the [Item Object](#item) table. For `bindings` and `communications`, you can select nested fields using dot notation: `bindings.entityId`.

Default: all chat fields ||
|#

For the `id` of an activity of another kind, such as a call, the method returns a "not found" error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.activity.openline.get`

{% endnote %}

The examples retrieve an Open Channel chat of an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8135,"select":["id","direction","lineId","lineName","connectorId","sessionId","chatId","clientVote","isCompleted","startTime","bindings"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.activity.openline.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8135,"select":["id","direction","lineId","lineName","connectorId","sessionId","chatId","clientVote","isCompleted","startTime","bindings"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.activity.openline.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type OpenLineGetResult = {
      item: {
        id: number
        direction: 'incoming' | 'outgoing' | 'undefined'
        lineId: number | null
        lineName: string | null
        connectorId: string | null
        sessionId: number | null
        chatId: number | null
        clientVote: number | null
        isCompleted: boolean
        startTime: string | null
        bindings: Array<{ entityTypeId: number; entityId: number }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<OpenLineGetResult>(
        'crm.smartprocess1038.activity.openline.get',
        {
          id: 8135,
          select: ['id', 'direction', 'lineId', 'lineName', 'connectorId', 'sessionId', 'chatId', 'clientVote', 'isCompleted', 'startTime', 'bindings'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Open line chat:', result.item)
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
      async function getOpenLineChat() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.activity.openline.get',
            {
              id: 8135,
              select: ['id', 'direction', 'lineId', 'lineName', 'connectorId', 'sessionId', 'chatId', 'clientVote', 'isCompleted', 'startTime', 'bindings'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Open line chat:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOpenLineChat)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.activity.openline.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 8135,
                "select": ["id", "direction", "lineId", "lineName", "connectorId", "sessionId", "chatId", "clientVote", "isCompleted", "startTime", "bindings"],
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
                'crm.smartprocess1038.activity.openline.get',
                [
                    'id' => 8135,
                    'select' => ['id', 'direction', 'lineId', 'lineName', 'connectorId', 'sessionId', 'chatId', 'clientVote', 'isCompleted', 'startTime', 'bindings'],
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
        'crm.smartprocess1038.activity.openline.get',
        {
            id: 8135,
            select: ['id', 'direction', 'lineId', 'lineName', 'connectorId', 'sessionId', 'chatId', 'clientVote', 'isCompleted', 'startTime', 'bindings'],
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
        'crm.smartprocess1038.activity.openline.get',
        [
            'id' => 8135,
            'select' => ['id', 'direction', 'lineId', 'lineName', 'connectorId', 'sessionId', 'chatId', 'clientVote', 'isCompleted', 'startTime', 'bindings'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.activity.openline.get", b24.Params{
    	"id":     8135,
    	"select": []string{"id", "direction", "lineId", "lineName", "connectorId", "sessionId", "chatId", "clientVote", "isCompleted", "startTime", "bindings"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.activity.openline.get: %w", err)
    }

    var result struct {
    	Item struct {
    		ID          int     `json:"id"`
    		Direction   string  `json:"direction"`
    		LineID      *int    `json:"lineId"`
    		LineName    *string `json:"lineName"`
    		ConnectorID *string `json:"connectorId"`
    		SessionID   *int    `json:"sessionId"`
    		ChatID      *int    `json:"chatId"`
    		ClientVote  *int    `json:"clientVote"`
    		IsCompleted bool    `json:"isCompleted"`
    		StartTime   *string `json:"startTime"`
    		Bindings    []struct {
    			EntityTypeID int `json:"entityTypeId"`
    			EntityID     int `json:"entityId"`
    		} `json:"bindings"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.ConnectorID, result.Item.ChatID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "direction": "incoming",
            "lineId": 17,
            "lineName": "Customer Support",
            "connectorId": "telegrambot",
            "sessionId": 791,
            "chatId": 4531,
            "clientVote": 5,
            "id": 8135,
            "isCompleted": true,
            "startTime": "2026-08-25T16:16:04+01:00",
            "bindings": [
                {
                    "entityTypeId": 1038,
                    "entityId": 41
                }
            ]
        }
    },
    "time": {
        "start": 1795430953,
        "finish": 1795430953.913458,
        "duration": 0.9134581089019775,
        "processing": 0,
        "date_start": "2026-11-23T13:49:13+01:00",
        "date_finish": "2026-11-23T13:49:13+01:00",
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
[`object`](../../../data-types.md) | Open Channel chat with the fields from `select`; without `select`, with all fields. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

All fields are read-only: chats are created and changed by Open Channels. The "Filter" and "Sorting" columns apply to the [crm.&lt;type&gt;.activity.openline.list](./crm-entity-activity-openline-list-rest-v3.md) method.

#|
|| **Name**
`type` | **Description** | **Filter** | **Sorting** ||
|| **id**
[`integer`](../../../data-types.md) | Chat ID as a CRM activity | + | + ||
|| **providerId**
[`string`](../../../data-types.md) | Activity provider, always `IMOPENLINES_SESSION` for chats | + | — ||
|| **isCompleted**
[`boolean`](../../../data-types.md) | Indicates that the chat is completed as an activity | + | — ||
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
[`datetime`](../../../data-types.md) | Chat start date and time | + | + ||
|| **endTime**
[`datetime`](../../../data-types.md) | Chat end date and time | — | + ||
|| **deadlineTime**
[`datetime`](../../../data-types.md) | Activity deadline, `null` if there is no deadline | + | + ||
|| **bindings**
[`object[]`](../../../data-types.md) | CRM items the chat is bound to. [Object description](#bindings) | — | — ||
|| **direction**
[`string`](../../../data-types.md) | Chat direction. Possible values:

- `incoming` — the client wrote first
- `outgoing` — the employee wrote first
- `undefined` — no direction | + | — ||
|| **communications**
[`object[]`](../../../data-types.md) | Chat participants. [Object description](#communications) | — | — ||
|| **lineId**
[`integer`](../../../data-types.md) | Open Channel ID. It is accepted by the [imopenlines.config.get](../../../imopenlines/openlines/imopenlines-config-get.md) method. `null` if the activity has neither a line nor a session | — | — ||
|| **lineName**
[`string`](../../../data-types.md) | Open Channel name, `null` if the line has been deleted | — | — ||
|| **connectorId**
[`string`](../../../data-types.md) | Channel the client wrote from, for example, `telegrambot`, `whatsappbyedna`, `livechat`. `null` if the session has been deleted | — | — ||
|| **sessionId**
[`integer`](../../../data-types.md) | Open Channel session ID. It is accepted by session methods, for example, [imopenlines.session.history.get](../../../imopenlines/openlines/sessions/imopenlines-session-history-get.md) | — | — ||
|| **chatId**
[`integer`](../../../data-types.md) | Session chat ID. To read the messages, pass it to the `im.dialog.*` methods as `chat<chatId>`, for example, to [im.dialog.messages.get](../../../chats/messages/im-dialog-messages-get.md). `null` if the session has been deleted | — | — ||
|| **clientVote**
[`integer`](../../../data-types.md) | Client rating of the session: `5` — liked, `1` — disliked. `null` if the client has not rated the session | — | — ||
|#

The `lineName`, `connectorId`, `chatId`, and `clientVote` fields are taken from the Open Channels data. If Bitrix24 has no Open Channels, these fields are `null`. The `lineId` and `sessionId` fields are stored in the activity and are returned in this case as well. If you have no access to the chat client, `sessionId`, `chatId`, and `clientVote` are `null`.

The method does not return chat messages: they are read by chat methods using `chatId`.

#### Bindings Object {#bindings}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | CRM object type ID, for example, `1` — lead, `2` — deal ||
|| **entityId**
[`integer`](../../../data-types.md) | CRM item ID ||
|#

#### Communications Object {#communications}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Communication channel type, `IM` for chats ||
|| **value**
[`string`](../../../data-types.md) | Client address in the Open Channel: the `imol` prefix, channel, line ID, client ID in the channel, and chat ID separated by vertical bars. If you have no access to the address, `null` is returned ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | CRM object type ID of the participant ||
|| **entityId**
[`integer`](../../../data-types.md) | CRM item ID of the participant ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 7989 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the chat `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|| `id` | Field value must be 1 or greater | Pass a positive ID ||
|#

#### Chat Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `7989` was not found | The activity does not exist, is not an Open Channel chat, the chat is not bound to an item of this object type, or you have no permission to view it. Check `id` with the [crm.&lt;type&gt;.activity.openline.list](./crm-entity-activity-openline-list-rest-v3.md) method and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `OpenLineActivityDto` | Check the field names in `select` against the [Item Object](#item) table. The `subject`, `description`, `type` fields and activity files are returned by the [crm.&lt;type&gt;.activity.get](./crm-entity-activity-get-rest-v3.md) method ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | For `crm.lead.activity.openline.get`: leads are disabled on Bitrix24. Enable leads or read the chat through another object type it is also bound to ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.deal.activity.openLine.get` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.activity.openline.get`. For an SPA, check `entityTypeId` with the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method. My company and B2E documents have no Open Channel chat methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-activity-openline-list-rest-v3.md)
- [{#T}](./crm-entity-activity-get-rest-v3.md)
- [{#T}](./crm-entity-activity-call-get-rest-v3.md)
- [{#T}](../../../imopenlines/openlines/sessions/imopenlines-session-history-get.md)
