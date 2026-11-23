# Get a List of Item Open Channel Chats v 3.0 crm.[type].activity.openline.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the CRM item whose chats are requested

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.activity.openline.list` method returns Open Channel chats bound to a single CRM item, with the Open Channel, channel, session, chat, and client rating. The object type is set by the `<type>` segment of the method name, and the item by the `ownerId` condition in the filter.

The list includes only Open Channel chats: the method skips activities of other kinds, and they do not take up space on the page. All activities of the item are returned by the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.activity.openline.list` ||
|| Lead | `crm.lead.activity.openline.list` ||
|| Contact | `crm.contact.activity.openline.list` ||
|| Company | `crm.company.activity.openline.list` ||
|| Estimate | `crm.quote.activity.openline.list` ||
|| Invoice | `crm.smartinvoice.activity.openline.list` ||
|| CRM document | `crm.smartdocument.activity.openline.list` ||
|| SPA | `crm.smartprocess<entityTypeId>.activity.openline.list`, for example, `crm.smartprocess1038.activity.openline.list` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs `entityTypeId` are returned by the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method.

The method name is written in lowercase: `crm.deal.activity.openline.list`, not `crm.deal.activity.openLine.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`. The `["ownerId","=",<item ID>]` condition is required, for example, `[["ownerId","=",41],["isCompleted","=",false]]`. [Parameter description](#filter) ||
|| **select**
[`array`](../../../data-types.md) | List of chat fields to return, for example, `["id","connectorId","sessionId","chatId"]`. The available fields are described in the [Items Element](#items) table. For `bindings` and `communications`, you can select nested fields using dot notation: `bindings.entityId`.

Default: all chat fields ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the `{"field": "direction"}` format. You can sort by the `id`, `createdTime`, `updatedTime`, `startTime`, `endTime`, and `deadlineTime` fields.

Possible direction values:

- `asc` — ascending
- `desc` — descending

Default: `{"id": "asc"}` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object. [Parameter description](#pagination) ||
|#

### Parameter filter {#filter}

The `ownerId` condition sets the item whose chats you want to retrieve. Rules for it:

- a single condition at the top level of the filter, with the `=` operator and a positive integer
- `ownerId` cannot be specified in a condition group, under negation, with another operator, twice, or outside `filter`
- if the item does not exist, is in the Recycle Bin, or you have no permission to view it, the method returns a "not found" error, not an empty list

The other conditions are combined with it by AND. You can filter by `id`, `providerId`, `isCompleted`, `assignedById`, `direction`, `createdTime`, `updatedTime`, `startTime`, `deadlineTime`. REST 3.0 operators and condition groups are supported; the syntax is described in the [REST 3.0 overview](../../../rest-v3.md#filter). A condition group is written with the `"type": "filter"` key: `{"type":"filter","logic":"or","conditions":[...]}`.

Fields from the Open Channels data — `lineId`, `lineName`, `connectorId`, `sessionId`, `chatId`, `clientVote` — cannot be used in filtering and sorting. For example, to find chats from Telegram, retrieve the item's chats and select the records with `connectorId` = `telegrambot` on your side.

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of chats per page. Values greater than `200` are reduced to `200`. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of chats to skip from the beginning of the list.

Default: `0` ||
|#

{% note info "" %}

The response does not contain the total number of chats. If a page returns fewer chats than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.activity.openline.list`

{% endnote %}

The examples retrieve Open Channel chats of an SPA item with the type ID `1038`, newest first. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",41]],"select":["id","direction","lineName","connectorId","sessionId","chatId","clientVote","isCompleted","startTime"],"order":{"startTime":"desc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.activity.openline.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",41]],"select":["id","direction","lineName","connectorId","sessionId","chatId","clientVote","isCompleted","startTime"],"order":{"startTime":"desc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.activity.openline.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type OpenLineListResult = {
      items: Array<{
        id: number
        direction: 'incoming' | 'outgoing' | 'undefined'
        lineName: string | null
        connectorId: string | null
        sessionId: number | null
        chatId: number | null
        clientVote: number | null
        isCompleted: boolean
        startTime: string | null
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<OpenLineListResult>(
        'crm.smartprocess1038.activity.openline.list',
        {
          filter: [
            ['ownerId', '=', 41],
          ],
          select: ['id', 'direction', 'lineName', 'connectorId', 'sessionId', 'chatId', 'clientVote', 'isCompleted', 'startTime'],
          order: {
            startTime: 'desc',
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
        console.info('Open line chats:', result.items)
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
      async function listOpenLineChats() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.activity.openline.list',
            {
              filter: [
                ['ownerId', '=', 41],
              ],
              select: ['id', 'direction', 'lineName', 'connectorId', 'sessionId', 'chatId', 'clientVote', 'isCompleted', 'startTime'],
              order: {
                startTime: 'desc',
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
          console.info('Open line chats:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listOpenLineChats)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.activity.openline.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["ownerId", "=", 41],
                ],
                "select": ["id", "direction", "lineName", "connectorId", "sessionId", "chatId", "clientVote", "isCompleted", "startTime"],
                "order": {
                    "startTime": "desc",
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.activity.openline.list',
                [
                    'filter' => [
                        ['ownerId', '=', 41],
                    ],
                    'select' => ['id', 'direction', 'lineName', 'connectorId', 'sessionId', 'chatId', 'clientVote', 'isCompleted', 'startTime'],
                    'order' => [
                        'startTime' => 'desc',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.activity.openline.list',
        {
            filter: [
                ['ownerId', '=', 41],
            ],
            select: ['id', 'direction', 'lineName', 'connectorId', 'sessionId', 'chatId', 'clientVote', 'isCompleted', 'startTime'],
            order: {
                startTime: 'desc',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.activity.openline.list',
        [
            'filter' => [
                ['ownerId', '=', 41],
            ],
            'select' => ['id', 'direction', 'lineName', 'connectorId', 'sessionId', 'chatId', 'clientVote', 'isCompleted', 'startTime'],
            'order' => [
                'startTime' => 'desc',
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
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.activity.openline.list", b24.Params{
    	"filter": []any{
    		[]any{"ownerId", "=", 41},
    	},
    	"select": []string{"id", "direction", "lineName", "connectorId", "sessionId", "chatId", "clientVote", "isCompleted", "startTime"},
    	"order": b24.Params{
    		"startTime": "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.activity.openline.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID          int     `json:"id"`
    		Direction   string  `json:"direction"`
    		LineName    *string `json:"lineName"`
    		ConnectorID *string `json:"connectorId"`
    		SessionID   *int    `json:"sessionId"`
    		ChatID      *int    `json:"chatId"`
    		ClientVote  *int    `json:"clientVote"`
    		IsCompleted bool    `json:"isCompleted"`
    		StartTime   *string `json:"startTime"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.ConnectorID, item.ChatID)
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
                "direction": "incoming",
                "lineName": "Customer Support",
                "connectorId": "telegrambot",
                "sessionId": 791,
                "chatId": 4531,
                "clientVote": 5,
                "id": 8135,
                "isCompleted": true,
                "startTime": "2026-08-25T16:16:04+01:00"
            },
            {
                "direction": "incoming",
                "lineName": "Website",
                "connectorId": "livechat",
                "sessionId": 783,
                "chatId": 2091,
                "clientVote": null,
                "id": 7593,
                "isCompleted": true,
                "startTime": "2026-07-24T17:35:12+01:00"
            }
        ]
    },
    "time": {
        "start": 1795431012,
        "finish": 1795431012.943746,
        "duration": 0.9437460899353027,
        "processing": 0,
        "date_start": "2026-11-23T13:50:12+01:00",
        "date_finish": "2026-11-23T13:50:12+01:00",
        "operating_reset_at": 1795431600,
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
|| **result.items**
[`object[]`](../../../data-types.md) | Array of chats with the fields from `select`; without `select`, with all fields. If the item has no matching chats, the array is empty. [Field description](#items) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

All fields are read-only: chats are created and changed by Open Channels.

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
[`object[]`](../../../data-types.md) | CRM items the chat is bound to: `entityTypeId` is the object type ID, `entityId` is the item ID | — | — ||
|| **direction**
[`string`](../../../data-types.md) | Chat direction. Possible values:

- `incoming` — the client wrote first
- `outgoing` — the employee wrote first
- `undefined` — no direction | + | — ||
|| **communications**
[`object[]`](../../../data-types.md) | Chat participants: `type` is the `IM` communication channel type, `value` is the client address in the Open Channel, `entityTypeId` and `entityId` are the participant's CRM item. If you have no access to the address, `value` is `null` | — | — ||
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

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "DTO \"OpenLineActivityDto\" in field \"connectorId\" requires attribute \"Filterable\" to perform this request.",
                "field": "connectorId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Required field `filter` is missing | Pass `filter` with a condition on `ownerId`. The error also occurs when `filter` is passed as a string ||
|| `ownerId` | ownerId is a condition of the filter: name it as ["ownerId", "=", N] there. | Remove `ownerId` from the request body and pass it as a condition in `filter` ||
|| `connectorId` | DTO `OpenLineActivityDto` in field `connectorId` requires attribute `Filterable` to perform this request | Filter only by the fields marked in the "Filter" column of the [Items Element](#items) table ||
|| `lineId` | DTO `OpenLineActivityDto` in field `lineId` requires attribute `Sortable` to perform this request | Sort only by `id`, `createdTime`, `updatedTime`, `startTime`, `endTime`, `deadlineTime` ||
|| `direction` | This request requires data type `Direction` in field `direction` | Pass a value from the `direction` field list: `incoming`, `outgoing`, `undefined` ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.ownerId` | Filter for required field `ownerId` is required | Add the `["ownerId","=",<item ID>]` condition to `filter` ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `["ownerId",">",41]` | Check the `ownerId` condition: a single one, at the top level, with the `=` operator. Write condition groups with the `"type": "filter"` key ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `999999999` was not found | The item from `ownerId` does not exist for this object type, is in the Recycle Bin, or you have no permission to view it. Check `ownerId` and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `phoneNumber` found in entity `OpenLineActivityDto` | Check the field names in `select`, `filter`, and `order` against the [Items Element](#items) table. The `subject`, `description`, `type` fields and activity files are returned by the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | For `crm.lead.activity.openline.list`: leads are disabled on Bitrix24. Enable leads or request chats of another object type ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.deal.activity.openLine.list` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.activity.openline.list`. For an SPA, check `entityTypeId` with the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method. My company and B2E documents have no Open Channel chat methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-activity-openline-get-rest-v3.md)
- [{#T}](./crm-entity-activity-list-rest-v3.md)
- [{#T}](./crm-entity-activity-call-list-rest-v3.md)
- [{#T}](../../../chats/messages/im-dialog-messages-get.md)
