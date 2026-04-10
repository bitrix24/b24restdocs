# Get the List of Chats im.recent.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.recent.list` retrieves a list of the user's recent conversations with pagination support.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **SKIP_OPENLINES**
[`string`](../data-types.md) | Skip open channel chats.

Possible values:
- `Y` — yes
- `N` — no ||
  || **SKIP_DIALOG**
  [`string`](../data-types.md) | Skip one-on-one dialogs.

Possible values:
- `Y` — yes
- `N` — no ||
  || **SKIP_CHAT**
  [`string`](../data-types.md) | Skip group chats.

Possible values:
- `Y` — yes
- `N` — no ||
  || **LAST_MESSAGE_DATE**
  [`datetime`](../data-types.md) | Date of the last item from the previous selection in [ATOM](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) (ISO-8601) format ||
  || **UNREAD_ONLY**
  [`string`](../data-types.md) | Return only dialogs with unread messages.

Possible values:
- `Y` — yes
- `N` — no ||
  || **PARSE_TEXT**
  [`string`](../data-types.md) | Parse the text of the last message.

Possible values:
- `Y` — yes
- `N` — no ||
  || **GET_ORIGINAL_TEXT**
  [`string`](../data-types.md) | Return the original text of the message without transformations.

Possible values:
- `Y` — yes
- `N` — no ||
  || **SKIP_UNDISTRIBUTED_OPENLINES**
  [`string`](../data-types.md) | Skip undistributed open channel chats.

Possible values:
- `Y` — yes
- `N` — no ||
  || **ONLY_COPILOT**
  [`string`](../data-types.md) | Return only BitrixGPT chats.

Possible values:
- `Y` — yes
- `N` — no ||
  || **ONLY_CHANNEL**
  [`string`](../data-types.md) | Return only channels.

Possible values:
- `Y` — yes
- `N` — no ||
  || **CAN_MANAGE_MESSAGES**
  [`string`](../data-types.md) | Return only chats with message management rights.

Possible values:
- `Y` — yes
- `N` — no ||
  || **OFFSET**
  [`integer`](../data-types.md) | Offset for pagination. Default: `0` ||
  || **LIMIT**
  [`integer`](../data-types.md) | Number of items per page. Default: `50`. Maximum value: `200` ||
  |#

## Pagination Recommendations

- To move to the next page, increase `OFFSET` by the value of `LIMIT` (`0`, `50`, `100`), not by the number of items in the response.

- The same dialogs may repeat between pages because the selection is initially built on internal records and then collapsed into unique dialogs. As a result, page boundaries may overlap.

- If open channel chats are not needed, pass `SKIP_OPENLINES = Y` — this reduces the likelihood of overlaps between pages.

- If the list is small, request it in a single call with an increased `LIMIT`, with a maximum value of `200`.

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"LAST_MESSAGE_DATE":"2026-02-25T18:30:00+01:00","SKIP_OPENLINES":"N","SKIP_DIALOG":"N","SKIP_CHAT":"N","UNREAD_ONLY":"Y","PARSE_TEXT":"Y","GET_ORIGINAL_TEXT":"N","SKIP_UNDISTRIBUTED_OPENLINES":"Y","ONLY_COPILOT":"N","ONLY_CHANNEL":"N","CAN_MANAGE_MESSAGES":"Y","OFFSET":0,"LIMIT":20}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.recent.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"LAST_MESSAGE_DATE":"2026-02-25T18:30:00+01:00","SKIP_OPENLINES":"N","SKIP_DIALOG":"N","SKIP_CHAT":"N","UNREAD_ONLY":"Y","PARSE_TEXT":"Y","GET_ORIGINAL_TEXT":"N","SKIP_UNDISTRIBUTED_OPENLINES":"Y","ONLY_COPILOT":"N","ONLY_CHANNEL":"N","CAN_MANAGE_MESSAGES":"Y","OFFSET":0,"LIMIT":20,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.recent.list
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.recent.list',
            {
                LAST_MESSAGE_DATE: '2026-02-25T18:30:00+01:00',
                SKIP_OPENLINES: 'N',
                SKIP_DIALOG: 'N',
                SKIP_CHAT: 'N',
                UNREAD_ONLY: 'Y',
                PARSE_TEXT: 'Y',
                GET_ORIGINAL_TEXT: 'N',
                SKIP_UNDISTRIBUTED_OPENLINES: 'Y',
                ONLY_COPILOT: 'N',
                ONLY_CHANNEL: 'N',
                CAN_MANAGE_MESSAGES: 'Y',
                OFFSET: 0,
                LIMIT: 20
            }
        );

        console.log(response.getData().result);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.recent.list',
                [
                    'LAST_MESSAGE_DATE' => '2026-02-25T18:30:00+01:00',
                    'SKIP_OPENLINES' => 'N',
                    'SKIP_DIALOG' => 'N',
                    'SKIP_CHAT' => 'N',
                    'UNREAD_ONLY' => 'Y',
                    'PARSE_TEXT' => 'Y',
                    'GET_ORIGINAL_TEXT' => 'N',
                    'SKIP_UNDISTRIBUTED_OPENLINES' => 'Y',
                    'ONLY_COPILOT' => 'N',
                    'ONLY_CHANNEL' => 'N',
                    'CAN_MANAGE_MESSAGES' => 'Y',
                    'OFFSET' => 0,
                    'LIMIT' => 20,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.recent.list',
        {
            LAST_MESSAGE_DATE: '2026-02-25T18:30:00+01:00',
            SKIP_OPENLINES: 'N',
            SKIP_DIALOG: 'N',
            SKIP_CHAT: 'N',
            UNREAD_ONLY: 'Y',
            PARSE_TEXT: 'Y',
            GET_ORIGINAL_TEXT: 'N',
            SKIP_UNDISTRIBUTED_OPENLINES: 'Y',
            ONLY_COPILOT: 'N',
            ONLY_CHANNEL: 'N',
            CAN_MANAGE_MESSAGES: 'Y',
            OFFSET: 0,
            LIMIT: 20
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.recent.list',
        [
            'LAST_MESSAGE_DATE' => '2026-02-25T18:30:00+01:00',
            'SKIP_OPENLINES' => 'N',
            'SKIP_DIALOG' => 'N',
            'SKIP_CHAT' => 'N',
            'UNREAD_ONLY' => 'Y',
            'PARSE_TEXT' => 'Y',
            'GET_ORIGINAL_TEXT' => 'N',
            'SKIP_UNDISTRIBUTED_OPENLINES' => 'Y',
            'ONLY_COPILOT' => 'N',
            'ONLY_CHANNEL' => 'N',
            'CAN_MANAGE_MESSAGES' => 'Y',
            'OFFSET' => 0,
            'LIMIT' => 20,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
	"result": {
		"items": [
			{
				"id": 547,
				"chat_id": 1231,
				"type": "user",
				"avatar": {
					"url": "",
					"color": "#1eb4aa"
				},
				"title": "John Doe",
				"message": {
					"id": 84415,
					"text": "ntcn",
					"file": false,
					"author_id": 547,
					"attach": false,
					"sticker": null,
					"date": "2026-02-25T12:07:10+01:00",
					"status": "received",
					"uuid": "0c3c5ad6-a1b8-4d4d-9234-f137ef862a77"
				},
				"counter": 1,
				"last_id": 82363,
				"pinned": false,
				"unread": false,
				"has_reminder": false,
				"date_update": "2026-02-25T12:07:10+01:00",
				"date_last_activity": "2026-02-25T12:07:10+01:00",
				"user": {
					"id": 547,
					"active": true,
					"name": "John Doe",
					"first_name": "John",
					"last_name": "Doe",
					"work_position": "Tester",
					"color": "#1eb4aa",
					"avatar": "",
					"avatar_hr": "/bitrix/js/im/images/blank.gif",
					"gender": "M",
					"birthday": "",
					"extranet": false,
					"network": false,
					"bot": false,
					"connector": false,
					"external_auth_id": "socservices",
					"status": "online",
					"idle": false,
					"last_activity_date": "2026-02-25T17:42:27+01:00",
					"mobile_last_date": false,
					"desktop_last_date": false,
					"absent": false,
					"departments": [
						1,
						667
					],
					"phones": false,
					"bot_data": null,
					"type": "user",
					"website": "",
					"email": "john.doe@mysite.com"
				},
				"chat": {
					"text_field_enabled": true,
					"background_id": null,
					"mute_list": []
				},
				"options": []
			},
			{
				"id": "chat1317",
				"chat_id": 1317,
				"type": "chat",
				"avatar": {
					"url": "https://cdn.com.bitrix24.com/b17053/resize_cache/54309/ff58db95aecdfa09ae61b51b5fd8f63f/im/553/5539ce72ef842ca40efd35b54322d828/7qd8lz4rsqwubjru086fst8tx6uhkxa9",
					"color": "#4ba984"
				},
				"title": "I want a green chat",
				"message": {
					"id": 84413,
					"text": "ntcn",
					"file": false,
					"author_id": 547,
					"attach": false,
					"sticker": null,
					"date": "2026-02-25T12:07:05+01:00",
					"status": "received",
					"uuid": "f4721d3d-68c6-473e-a899-3b96b0ee18db"
				},
				"counter": 1,
				"last_id": 82447,
				"pinned": false,
				"unread": false,
				"has_reminder": false,
				"date_update": "2026-02-25T12:07:05+01:00",
				"date_last_activity": "2026-02-25T12:07:05+01:00",
				"chat": {
					"id": 1317,
					"parent_chat_id": 0,
					"parent_message_id": 0,
					"name": "I want a green chat",
					"owner": 547,
					"extranet": false,
					"contains_collaber": false,
					"avatar": "https://cdn.com.bitrix24.com/b17053/resize_cache/54309/ff58db95aecdfa09ae61b51b5fd8f63f/im/553/5539ce72ef842ca40efd35b54322d828/7qd8lz4rsqwubjru086fst8tx6uhkxa9",
					"color": "#4ba984",
					"type": "chat",
					"entity_type": "",
					"entity_id": "",
					"entity_data_1": "",
					"entity_data_2": "",
					"entity_data_3": "",
					"mute_list": {
						"503": true
					},
					"manager_list": [],
					"date_create": "2025-08-12T16:14:00+01:00",
					"message_type": "C",
					"user_counter": 2,
					"restrictions": {
						"avatar": true,
						"rename": true,
						"extend": true,
						"call": true,
						"mute": true,
						"leave": true,
						"leave_owner": true,
						"send": true,
						"user_list": true
					},
					"role": "MEMBER",
					"text_field_enabled": true,
					"background_id": null,
					"entity_link": {
						"type": "",
						"url": "",
						"id": ""
					},
					"permissions": {
						"manage_users_add": "member",
						"manage_users_delete": "manager",
						"manage_ui": "member",
						"manage_settings": "owner",
						"manage_messages": "member",
						"can_post": "member"
					},
					"public": ""
				},
				"user": {
					"id": 547,
					"active": true,
					"name": "John Doe",
					"first_name": "John",
					"last_name": "Doe",
					"work_position": "Tester",
					"color": "#1eb4aa",
					"avatar": "",
					"avatar_hr": "/bitrix/js/im/images/blank.gif",
					"gender": "M",
					"birthday": "",
					"extranet": false,
					"network": false,
					"bot": false,
					"connector": false,
					"external_auth_id": "socservices",
					"status": "online",
					"idle": false,
					"last_activity_date": "2026-02-25T17:42:27+01:00",
					"mobile_last_date": false,
					"desktop_last_date": false,
					"absent": false,
					"departments": [
						1,
						667
					],
					"phones": false,
					"bot_data": null,
					"type": "user",
					"website": "",
					"email": "john.doe@mysite.com"
				},
				"options": []
			}
		],
		"hasMorePages": false,
		"hasMore": false,
		"copilot": {
			"chats": null,
			"messages": null,
			"roles": {
				"copilot_assistant": {
					"code": "copilot_assistant",
					"name": "BitrixGPT",
					"desc": "Ready to answer all questions in a general format",
					"avatar": {
						"small": "https://preview.bitrix24.site/upload/ai/avatars/5f72dd53304450356e0eaf09c0fcda7b_64x64.png",
						"medium": "https://preview.bitrix24.site/upload/ai/avatars/f62609ab98ede5b3d4bff7675cc9f1f5_128x128.png",
						"large": "https://preview.bitrix24.site/upload/ai/avatars/0a89c642ed5f1c2a292f7c3a1eab99d3_256x256.png"
					},
					"default": true,
					"prompts": [
						{
							"code": "universal_how_to_properly_write_a_business_letter",
							"promptType": "default",
							"title": "How to properly write a business letter?",
							"text": "How to properly write a business letter?",
							"isNew": false
						},
						{
							"code": "universal_effective_methods_to_combat_procrastination_in_the_workplace",
							"promptType": "default",
							"title": "How to combat procrastination?",
							"text": "Tell me about effective methods to combat procrastination in the workplace",
							"isNew": false
						},
						{
							"code": "universal_ideas_on_how_to_make_meetings_more_concise_and_substantive",
							"promptType": "default",
							"title": "Ideas for meetings",
							"text": "Do you have ideas on how to make meetings more concise and substantive?",
							"isNew": false
						},
						{
							"code": "universal_ideas_for_short_breaks_for_physical_exercises",
							"promptType": "default",
							"title": "Ideas for short breaks",
							"text": "Suggest ideas for short breaks for physical exercises in the office",
							"isNew": false
						}
					]
				},
				... // description of other roles
			},
			"recommendedRoles": [
				"copilot_assistant",
				"smm_manager",
				"seo_copywriter",
				"prompt_generator",
				"marketing_specialist"
			]
		},
		"messagesAutoDeleteConfigs": []
	},
	"total": -1,
	"time": {
		"start": 1772089843,
		"finish": 1772089843.789026,
		"duration": 0.7890260219573975,
		"processing": 0,
		"date_start": "2026-02-26T10:10:43+01:00",
		"date_finish": "2026-02-26T10:10:43+01:00",
		"operating_reset_at": 1772090443,
		"operating": 0.2634410858154297
	}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root object of the result [(detailed description)](#result-item) ||
|| **total**
[`integer`](../data-types.md) | Total number of items. In the current implementation, usually `-1` ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Object result-item {#result-item}

#|
|| **Name**
`type` | **Description** ||
|| **items**
[`array`](../data-types.md) | List of recent dialogs [(detailed description)](#items-item) ||
|| **hasMorePages**
[`boolean`](../data-types.md) | Deprecated alias for the field `hasMore` ||
|| **hasMore**
[`boolean`](../data-types.md) | Indicator of the presence of the next page of the selection ||
|| **copilot**
[`object`](../data-types.md) | Additional data for BitrixGPT elements [(detailed description)](#copilot) ||
|| **messagesAutoDeleteConfigs**
[`array`](../data-types.md) | Auto-deletion settings for messages by chats ||
|#

#### Object items {#items-item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Identifier of the dialog: number for user, `chatXXX` for chat ||
|| **chat_id**
[`integer`](../data-types.md) | Identifier of the chat ||
|| **type**
[`string`](../data-types.md) | Type of record: `user` or `chat` ||
|| **avatar**
[`object`](../data-types.md) | Avatar object [(detailed description)](#items-item-avatar) ||
|| **title**
[`string`](../data-types.md) | Title of the record: user's name or chat name ||
|| **message**
[`object`](../data-types.md) | Last message in the dialog [(detailed description)](#items-item-message) ||
|| **counter**
[`integer`](../data-types.md) | Counter of unread messages ||
|| **last_id**
[`integer`](../data-types.md) | Identifier of the last read message ||
|| **pinned**
[`boolean`](../data-types.md) | Indicator of a pinned dialog ||
|| **unread**
[`boolean`](../data-types.md) | Indicator of a manual "unread" mark ||
|| **has_reminder**
[`boolean`](../data-types.md) | Indicator of a set reminder ||
|| **date_update**
[`datetime`](../data-types.md) | Date of the last change in the dialog in ISO 8601 format ||
|| **date_last_activity**
[`datetime`](../data-types.md) | Date of the last activity in the dialog in ISO 8601 format ||
|| **user**
[`object`](../data-types.md) | User data [(detailed description)](#items-item-user) ||
|| **chat**
[`object`](../data-types.md) | Chat data [(detailed description)](#items-item-chat) ||
|| **lines**
[`object`](../data-types.md) | Open line data [(detailed description)](#items-item-lines) ||
|| **options**
[`array`](../data-types.md) | Additional parameters of the record ||
|#

#### Object avatar {#items-item-avatar}

#|
|| **Name**
`type` | **Description** ||
|| **url**
[`string`](../data-types.md) | Link to the avatar. If empty, the avatar is not set ||
|| **color**
[`string`](../data-types.md) | Color of the dialog in HEX format ||
|#

#### Object message {#items-item-message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the message ||
|| **text**
[`string`](../data-types.md) | Text of the message ||
|| **file**
[`boolean`](../data-types.md) | Indicator of the presence of files ||
|| **author_id**
[`integer`](../data-types.md) | Identifier of the message author ||
|| **attach**
[`boolean`](../data-types.md) | Indicator of the presence of attachments ||
|| **date**
[`datetime`](../data-types.md) | Date of the message in ATOM format ||
|| **status**
[`string`](../data-types.md) | Status of message delivery ||
|| **sticker**
[`integer`](../data-types.md) | Identifier of the sticker. If there is no sticker, the value is `null` ||
|| **uuid**
[`string`](../data-types.md) | External identifier of the message. If not set, the value is `null` ||
|#

#### Object user {#items-item-user}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the user ||
|| **active**
[`boolean`](../data-types.md) | Indicator of an active user ||
|| **name**
[`string`](../data-types.md) | User's full name ||
|| **first_name**
[`string`](../data-types.md) | User's first name ||
|| **last_name**
[`string`](../data-types.md) | User's last name ||
|| **work_position**
[`string`](../data-types.md) | User's position ||
|| **color**
[`string`](../data-types.md) | User's color in HEX format ||
|| **avatar**
[`string`](../data-types.md) | Link to the avatar ||
|| **avatar_hr**
[`string`](../data-types.md) | Link to high-resolution avatar ||
|| **gender**
[`string`](../data-types.md) | User's gender ||
|| **birthday**
[`string`](../data-types.md) | Birthday in `DD-MM` format or empty string ||
|| **extranet**
[`boolean`](../data-types.md) | Indicator of an external extranet user ||
|| **network**
[`boolean`](../data-types.md) | Indicator of a Bitrix24.Network user ||
|| **bot**
[`boolean`](../data-types.md) | Indicator of a bot ||
|| **connector**
[`boolean`](../data-types.md) | Indicator of an open line user ||
|| **external_auth_id**
[`string`](../data-types.md) | External authorization code ||
|| **status**
[`string`](../data-types.md) | User's status ||
|| **idle**
[`datetime`](../data-types.md) | Date when the user stepped away from the computer. If not set, `false` ||
|| **last_activity_date**
[`datetime`](../data-types.md) | Date of the user's last activity ||
|| **mobile_last_date**
[`datetime`](../data-types.md) | Date of the last activity in the mobile application. If not set, `false` ||
|| **desktop_last_date**
[`datetime`](../data-types.md) | Date of the last activity in the desktop application. If not set, `false` ||
|| **absent**
[`datetime`](../data-types.md) | Date of the user's vacation. If not set, `false` ||
|| **departments**
[`array`](../data-types.md) | List of user department identifiers ||
|| **phones**
[`object`](../data-types.md) | User contact phones. Can be `false` ||
|| **bot_data**
[`object`](../data-types.md) | Bot data. For a regular user, it can be `null` ||
|| **type**
[`string`](../data-types.md) | Type of user ||
|| **website**
[`string`](../data-types.md) | User's website ||
|| **email**
[`string`](../data-types.md) | User's email ||
|#

#### Object chat {#items-item-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the chat ||
|| **parent_chat_id**
[`integer`](../data-types.md) | Identifier of the parent chat ||
|| **parent_message_id**
[`integer`](../data-types.md) | Identifier of the parent message ||
|| **name**
[`string`](../data-types.md) | Name of the chat ||
|| **owner**
[`integer`](../data-types.md) | Identifier of the chat owner ||
|| **extranet**
[`boolean`](../data-types.md) | Indicator of the participation of an external extranet user ||
|| **contains_collaber**
[`boolean`](../data-types.md) | Indicator of the participation of collab users ||
|| **avatar**
[`string`](../data-types.md) | Link to the avatar. If empty, the avatar is not set ||
|| **color**
[`string`](../data-types.md) | Color of the chat in HEX format ||
|| **type**
[`string`](../data-types.md) | Type of chat ||
|| **entity_type**
[`string`](../data-types.md) | External code of the chat: type ||
|| **entity_id**
[`string`](../data-types.md) | External code of the chat: identifier ||
|| **entity_data_1**
[`string`](../data-types.md) | External data 1 for the chat ||
|| **entity_data_2**
[`string`](../data-types.md) | External data 2 for the chat ||
|| **entity_data_3**
[`string`](../data-types.md) | External data 3 for the chat ||
|| **mute_list**
[`array`](../data-types.md) | List of users with notifications disabled ||
|| **manager_list**
[`array`](../data-types.md) | List of chat manager identifiers ||
|| **date_create**
[`datetime`](../data-types.md) | Date of chat creation in ATOM format ||
|| **message_type**
[`string`](../data-types.md) | Type of chat messages ||
|| **user_counter**
[`integer`](../data-types.md) | Number of chat participants ||
|| **restrictions**
[`object`](../data-types.md) | Restrictions on actions in the chat [(detailed description)](#items-item-chat-restrictions) ||
|| **role**
[`string`](../data-types.md) | Current user's role in the chat ||
|| **text_field_enabled**
[`boolean`](../data-types.md) | Availability of the message input field ||
|| **background_id**
[`integer`](../data-types.md) | Identifier of the chat background. If not set, the value is `null` ||
|| **entity_link**
[`object`](../data-types.md) | Link to the related object [(detailed description)](#items-item-chat-entity-link) ||
|| **permissions**
[`object`](../data-types.md) | Permissions for actions in the chat [(detailed description)](#items-item-chat-permissions) ||
|| **public**
[`string`](../data-types.md) | Indicator of the chat's public status ||
|#

#### Object lines {#items-item-lines}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the open line ||
|| **status**
[`integer`](../data-types.md) | Status of the open line ||
|| **date_create**
[`datetime`](../data-types.md) | Date of open line creation in ATOM format ||
|#

#### Object restrictions {#items-item-chat-restrictions}

#|
|| **Name**
`type` | **Description** ||
|| **avatar**
[`boolean`](../data-types.md) | Availability of avatar change ||
|| **rename**
[`boolean`](../data-types.md) | Availability of name change ||
|| **extend**
[`boolean`](../data-types.md) | Availability of chat extension ||
|| **call**
[`boolean`](../data-types.md) | Availability of calls ||
|| **mute**
[`boolean`](../data-types.md) | Availability of notifications mute ||
|| **leave**
[`boolean`](../data-types.md) | Availability of leaving the chat ||
|| **leave_owner**
[`boolean`](../data-types.md) | Availability of the owner leaving the chat ||
|| **send**
[`boolean`](../data-types.md) | Availability of message sending ||
|| **user_list**
[`boolean`](../data-types.md) | Availability of viewing the list of participants ||
|#

#### Object entity_link {#items-item-chat-entity-link}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../data-types.md) | Type of the related object ||
|| **url**
[`string`](../data-types.md) | Link to the related object ||
|| **id**
[`string`](../data-types.md) | Identifier of the related object ||
|#

#### Object permissions {#items-item-chat-permissions}

#|
|| **Name**
`type` | **Description** ||
|| **manage_users_add**
[`string`](../data-types.md) | Permission to add participants ||
|| **manage_users_delete**
[`string`](../data-types.md) | Permission to remove participants ||
|| **manage_ui**
[`string`](../data-types.md) | Permission to manage the chat interface ||
|| **manage_settings**
[`string`](../data-types.md) | Permission to manage chat settings ||
|| **manage_messages**
[`string`](../data-types.md) | Permission to manage messages ||
|| **can_post**
[`string`](../data-types.md) | Permission to send messages ||
|#

### Object copilot {#copilot}

#|
|| **Name**
`type` | **Description** ||
|| **chats**
[`object`](../data-types.md) | Data for BitrixGPT chats. Can be `null` ||
|| **messages**
[`object`](../data-types.md) | Data for BitrixGPT messages. Can be `null` ||
|| **roles**
[`object`](../data-types.md) | Description of available BitrixGPT roles ||
|| **recommendedRoles**
[`array`](../data-types.md) | List of recommended BitrixGPT roles ||
|#

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-recent-get.md)
- [{#T}](./im-dialog-get.md)
- [{#T}](./im-counters-get.md)