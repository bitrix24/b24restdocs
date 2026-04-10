# Get the List of Open Channels imopenlines.config.list.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imopenlines.config.list.get` retrieves a list of open channels.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PARAMS**
[`object`](../../data-types.md) | Selection parameters [(detailed description)](#params)
||
|| **OPTIONS**
[`object`](../../data-types.md) | Additional options [(detailed description)](#options) ||
|#

### PARAMS Parameter {#params}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the [list of fields](#result) to be selected.

You can specify fields from the [Result Array Element](#result) table. The fields `QUEUE`, `QUEUE_USERS_FIELDS`, `CONFIG_QUEUE` are returned only through `OPTIONS` ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the list of open channels in the format `{"field_1": "value_1", ... "field_N": "value_N"}`

The sorting direction can take the following values:

- `asc` — ascending
- `desc` — descending

For `field_N`, use fields from the [Result Array Element](#result) table. The fields `QUEUE`, `QUEUE_USERS_FIELDS`, `CONFIG_QUEUE` are not supported in `order` ||

|| **filter**
[`object`](../../data-types.md) | An object for filtering the list of open channels in the format `{"field_1": "value_1", ... "field_N": "value_N"}`

For `field_N`, use fields from the [Result Array Element](#result) table. The fields `QUEUE`, `QUEUE_USERS_FIELDS`, `CONFIG_QUEUE` are not supported in `filter` ||
|| **limit**
[`integer`](../../data-types.md) | The number of items per page. Default: `50`. Maximum value: `200` ||
|| **offset**
[`integer`](../../data-types.md) | Offset for pagination. Default: `0` ||
|#

### OPTIONS Parameter {#options}

#|
|| **Name**
`type` | **Description** ||
|| **QUEUE**
[`char`](../../data-types.md) | Return the operator queue. Possible values:
- `Y` — yes
- `N` — no ||
|| **CONFIG_QUEUE**
[`char`](../../data-types.md) | Return the line queue. Each queue element contains `ENTITY_TYPE` and `ENTITY_ID`. Possible values:
- `Y` — yes
- `N` — no ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PARAMS": {
          "select": ["ID", "LINE_NAME", "ACTIVE"],
          "order": {"ID": "ASC"},
          "filter": {"ACTIVE": "Y"},
          "limit": 50,
          "offset": 0
        },
        "OPTIONS": {
          "QUEUE": "Y",
          "CONFIG_QUEUE": "Y"
        }
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.config.list.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PARAMS": {
          "select": ["ID", "LINE_NAME", "ACTIVE"],
          "order": {"ID": "ASC"},
          "filter": {"ACTIVE": "Y"},
          "limit": 50,
          "offset": 0
        },
        "OPTIONS": {
          "QUEUE": "Y",
          "CONFIG_QUEUE": "Y"
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.config.list.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.config.list.get',
            {
                PARAMS: {
                    select: ['ID', 'LINE_NAME', 'ACTIVE'],
                    order: { ID: 'ASC' },
                    filter: { ACTIVE: 'Y' },
                    limit: 50,
                    offset: 0
                },
                OPTIONS: {
                    QUEUE: 'Y',
                    CONFIG_QUEUE: 'Y'
                }
            }
        );

        const result = response.getData().result;
        console.log(result);
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
                'imopenlines.config.list.get',
                [
                    'PARAMS' => [
                        'select' => ['ID', 'LINE_NAME', 'ACTIVE'],
                        'order' => ['ID' => 'ASC'],
                        'filter' => ['ACTIVE' => 'Y'],
                        'limit' => 50,
                        'offset' => 0,
                    ],
                    'OPTIONS' => [
                        'QUEUE' => 'Y',
                        'CONFIG_QUEUE' => 'Y',
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

    ```js
    BX24.callMethod(
        'imopenlines.config.list.get',
        {
            PARAMS: {
                select: ['ID', 'LINE_NAME', 'ACTIVE'],
                order: { ID: 'ASC' },
                filter: { ACTIVE: 'Y' },
                limit: 50,
                offset: 0
            },
            OPTIONS: {
                QUEUE: 'Y',
                CONFIG_QUEUE: 'Y'
            }
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
        'imopenlines.config.list.get',
        [
            'PARAMS' => [
                'select' => ['ID', 'LINE_NAME', 'ACTIVE'],
                'order' => ['ID' => 'ASC'],
                'filter' => ['ACTIVE' => 'Y'],
                'limit' => 50,
                'offset' => 0,
            ],
            'OPTIONS' => [
                'QUEUE' => 'Y',
                'CONFIG_QUEUE' => 'Y',
            ],
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "15",
            "LINE_NAME": "VIP Support Line",
            "ACTIVE": "Y",
            "QUEUE": [101, 205],
            "QUEUE_USERS_FIELDS": {
                "101": {
                    "USER_NAME": "John Smith",
                    "USER_WORK_POSITION": "Operator",
                    "USER_AVATAR": "https://example.bitrix24.com/upload/main/a1b/avatar.jpg",
                    "USER_AVATAR_ID": 312
                },
                "205": {
                    "USER_NAME": "Anna Johnson",
                    "USER_WORK_POSITION": null,
                    "USER_AVATAR": "",
                    "USER_AVATAR_ID": null
                }
            },
            "CONFIG_QUEUE": [
                {
                    "ENTITY_ID": "10",
                    "ENTITY_TYPE": "department"
                }
            ]
        },
        {
            "ID": "16",
            "LINE_NAME": "Sales Line",
            "ACTIVE": "Y",
            "QUEUE": [101],
            "QUEUE_USERS_FIELDS": {
                "101": {
                    "USER_NAME": "John Smith",
                    "USER_WORK_POSITION": "Operator",
                    "USER_AVATAR": "https://example.bitrix24.com/upload/main/a1b/avatar.jpg",
                    "USER_AVATAR_ID": 312
                }
            },
            "CONFIG_QUEUE": []
        }
    ],
    "time": {
        "start": 1741688574.50636,
        "finish": 1741688574.701981,
        "duration": 0.19562101364135742,
        "processing": 0.09473705291748047,
        "date_start": "2025-03-11T10:29:34+01:00",
        "date_finish": "2025-03-11T10:29:34+01:00",
        "operating_reset_at": 1741689174,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of open channels [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Array Element {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Identifier of the open channel ||
|| **LINE_NAME**
[`string`](../../data-types.md) | Name of the open channel ||
|| **ACTIVE**
[`string`](../../data-types.md) | Activity status of the line. Possible values:
- `Y` — line is active
- `N` — line is inactive ||
|| **CRM**
[`string`](../../data-types.md) | CRM interaction status. Possible values:
- `Y` — yes
- `N` — no ||
|| **CRM_CREATE**
[`string`](../../data-types.md) | Scenario for creating a CRM element ||
|| **CRM_CREATE_SECOND**
[`string`](../../data-types.md) | Additional mode for creating a CRM element ||
|| **CRM_CREATE_THIRD**
[`string`](../../data-types.md) | Additional mode for creating a CRM element. Possible values:
- `Y` — yes
- `N` — no ||
|| **CRM_FORWARD**
[`string`](../../data-types.md) | Status of forwarding to CRM. Possible values:
- `Y` — yes
- `N` — no ||
|| **CRM_CHAT_TRACKER**
[`string`](../../data-types.md) | Status of sending the dialog to the CRM tracker. Possible values:
- `Y` — yes
- `N` — no ||
|| **CRM_TRANSFER_CHANGE**
[`string`](../../data-types.md) | Status of changing the responsible person during transfer. Possible values:
- `Y` — yes
- `N` — no ||
|| **CRM_SOURCE**
[`string`](../../data-types.md) | Mode of working with the source in CRM ||
|| **QUEUE_TYPE**
[`string`](../../data-types.md) | Mode of distributing requests ||
|| **QUEUE_TIME**
[`string`](../../data-types.md) | Time to transfer the request to the next operator ||
|| **NO_ANSWER_TIME**
[`string`](../../data-types.md) | Time until the no-answer scenario is triggered ||
|| **CHECK_AVAILABLE**
[`string`](../../data-types.md) | Check the availability of operators. Possible values:
- `Y` — yes
- `N` — no ||
|| **WATCH_TYPING**
[`string`](../../data-types.md) | Show text being typed by the operator. Possible values:
- `Y` — yes
- `N` — no ||
|| **WELCOME_BOT_ENABLE**
[`string`](../../data-types.md) | Status of using the welcome bot. Possible values:
- `Y` — yes
- `N` — no ||
|| **WELCOME_MESSAGE**
[`string`](../../data-types.md) | Status of the welcome message. Possible values:
- `Y` — yes
- `N` — no ||
|| **WELCOME_MESSAGE_TEXT**
[`string`](../../data-types.md) | Text of the welcome message ||
|| **VOTE_MESSAGE**
[`string`](../../data-types.md) | Status of the feedback request. Possible values:
- `Y` — yes
- `N` — no ||
|| **VOTE_TIME_LIMIT**
[`string`](../../data-types.md) | Time limit for feedback ||
|| **VOTE_BEFORE_FINISH**
[`string`](../../data-types.md) | Request feedback before the dialog ends. Possible values:
- `Y` — yes
- `N` — no ||
|| **VOTE_CLOSING_DELAY**
[`string`](../../data-types.md) | Use a delay for closing after feedback. Possible values:
- `Y` — yes
- `N` — no ||
|| **VOTE_MESSAGE_1_TEXT**
[`string`](../../data-types.md) | Text of the first feedback scenario ||
|| **VOTE_MESSAGE_1_LIKE**
[`string`](../../data-types.md) | Text for positive feedback in the first scenario ||
|| **VOTE_MESSAGE_1_DISLIKE**
[`string`](../../data-types.md) | Text for negative feedback in the first scenario ||
|| **VOTE_MESSAGE_2_TEXT**
[`string`](../../data-types.md) | Text of the second feedback scenario ||
|| **VOTE_MESSAGE_2_LIKE**
[`string`](../../data-types.md) | Text for positive feedback in the second scenario ||
|| **VOTE_MESSAGE_2_DISLIKE**
[`string`](../../data-types.md) | Text for negative feedback in the second scenario ||
|| **AGREEMENT_MESSAGE**
[`string`](../../data-types.md) | Status of displaying the agreement message. Possible values:
- `Y` — yes
- `N` — no ||
|| **AGREEMENT_ID**
[`string`](../../data-types.md) | Identifier of the agreement ||
|| **CATEGORY_ENABLE**
[`string`](../../data-types.md) | Status of using categories. Possible values:
- `Y` — yes
- `N` — no ||
|| **CATEGORY_ID**
[`string`](../../data-types.md) | Identifier of the category ||
|| **WELCOME_BOT_JOIN**
[`string`](../../data-types.md) | Mode of connecting the welcome bot ||
|| **WELCOME_BOT_ID**
[`string`](../../data-types.md) | Identifier of the welcome bot ||
|| **WELCOME_BOT_TIME**
[`string`](../../data-types.md) | Time to wait for the welcome bot connection ||
|| **WELCOME_BOT_LEFT**
[`string`](../../data-types.md) | Mode of disconnecting the welcome bot ||
|| **NO_ANSWER_RULE**
[`string`](../../data-types.md) | Scenario for no response from the operator ||
|| **NO_ANSWER_FORM_ID**
[`string`](../../data-types.md) | Identifier of the no-answer scenario form ||
|| **NO_ANSWER_BOT_ID**
[`string`](../../data-types.md) | Identifier of the no-answer scenario bot ||
|| **NO_ANSWER_TEXT**
[`string`](../../data-types.md) | Text message for no response ||
|| **WORKTIME_ENABLE**
[`string`](../../data-types.md) | Status of considering working hours. Possible values:
- `Y` — yes
- `N` — no ||
|| **WORKTIME_FROM**
[`string`](../../data-types.md) | Start of working hours ||
|| **WORKTIME_TO**
[`string`](../../data-types.md) | End of working hours ||
|| **WORKTIME_TIMEZONE**
[`string`](../../data-types.md) | Working hours time zone ||
|| **WORKTIME_HOLIDAYS**
[`array`](../../data-types.md) | Array of holidays ||
|| **WORKTIME_DAYOFF**
[`array`](../../data-types.md) | Array of days off ||
|| **WORKTIME_DAYOFF_RULE**
[`string`](../../data-types.md) | Scenario for handling requests during non-working hours ||
|| **WORKTIME_DAYOFF_FORM_ID**
[`string`](../../data-types.md) | Identifier of the non-working hours scenario form ||
|| **WORKTIME_DAYOFF_BOT_ID**
[`string`](../../data-types.md) | Identifier of the non-working hours scenario bot ||
|| **WORKTIME_DAYOFF_TEXT**
[`string`](../../data-types.md) | Text message during non-working hours ||
|| **CLOSE_RULE**
[`string`](../../data-types.md) | Scenario for closing the dialog ||
|| **CLOSE_FORM_ID**
[`string`](../../data-types.md) | Identifier of the closing form ||
|| **CLOSE_BOT_ID**
[`string`](../../data-types.md) | Identifier of the closing bot ||
|| **CLOSE_TEXT**
[`string`](../../data-types.md) | Text message upon closing ||
|| **FULL_CLOSE_TIME**
[`string`](../../data-types.md) | Time for full session closure ||
|| **AUTO_CLOSE_RULE**
[`string`](../../data-types.md) | Auto-closing scenario ||
|| **AUTO_CLOSE_FORM_ID**
[`string`](../../data-types.md) | Identifier of the auto-closing form ||
|| **AUTO_CLOSE_BOT_ID**
[`string`](../../data-types.md) | Identifier of the auto-closing bot ||
|| **AUTO_CLOSE_TIME**
[`string`](../../data-types.md) | Time until auto-closing ||
|| **AUTO_CLOSE_TEXT**
[`string`](../../data-types.md) | Text for auto-closing ||
|| **AUTO_EXPIRE_TIME**
[`string`](../../data-types.md) | Time until activity expiration ||
|| **DATE_CREATE**
[`object`](../../data-types.md) | Date of setting creation in serialized form ||
|| **DATE_MODIFY**
[`object`](../../data-types.md) | Date of setting modification in serialized form ||
|| **MODIFY_USER_ID**
[`string`](../../data-types.md) | Identifier of the user who modified the settings ||
|| **TEMPORARY**
[`string`](../../data-types.md) | Status of temporary line. Possible values:
- `Y` — temporary line
- `N` — permanent line ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier. Possible values:
- `string` — external identifier is set
- `null` — external identifier is not set ||
|| **LANGUAGE_ID**
[`string`](../../data-types.md) | Language of the line ||
|| **QUICK_ANSWERS_IBLOCK_ID**
[`string`](../../data-types.md) | Identifier of the quick answers information block ||
|| **SESSION_PRIORITY**
[`string`](../../data-types.md) | Session priority ||
|| **TYPE_MAX_CHAT**
[`string`](../../data-types.md) | Mode of limiting active dialogs per operator ||
|| **MAX_CHAT**
[`string`](../../data-types.md) | Maximum active dialogs per operator ||
|| **OPERATOR_DATA**
[`string`](../../data-types.md) | Mode of displaying operator data ||
|| **QUEUE**
[`array`](../../data-types.md) | Identifiers of operators in the queue. 

Field is returned when `OPTIONS.QUEUE = "Y"` ||
|| **QUEUE_USERS_FIELDS**
[`object`](../../data-types.md) | Additional fields of queue users. Key — user identifier, value — description of queue fields [(detailed description)](#queue-users-fields-item). 

Field is returned when `OPTIONS.QUEUE = "Y"`  ||
|| **CONFIG_QUEUE**
[`array`](../../data-types.md) | Ordered list of line queue objects [(detailed description)](#config-queue-item-fields).

Field is returned when `OPTIONS.CONFIG_QUEUE = "Y"`  ||
|| **DEFAULT_OPERATOR_DATA**
[`array`](../../data-types.md) | Set of default operator display fields ||
|| **KPI_FIRST_ANSWER_TIME**
[`string`](../../data-types.md) | Standard time for the first response ||
|| **KPI_FIRST_ANSWER_ALERT**
[`string`](../../data-types.md) | Status of notification for overdue first response. Possible values:
- `Y` — yes
- `N` — no ||
|| **KPI_FIRST_ANSWER_LIST**
[`array`](../../data-types.md) | List of recipients for first response notifications. Possible values:
- `array` — list of recipients is set
- `null` — list of recipients is not set ||
|| **KPI_FIRST_ANSWER_TEXT**
[`string`](../../data-types.md) | Text of notification for overdue first response ||
|| **KPI_FURTHER_ANSWER_TIME**
[`string`](../../data-types.md) | Standard time for subsequent responses ||
|| **KPI_FURTHER_ANSWER_ALERT**
[`string`](../../data-types.md) | Status of notification for overdue subsequent responses. Possible values:
- `Y` — yes
- `N` — no ||
|| **KPI_FURTHER_ANSWER_LIST**
[`array`](../../data-types.md) | List of recipients for subsequent response notifications. Possible values:
- `array` — list of recipients is set
- `null` — list of recipients is not set ||
|| **KPI_FURTHER_ANSWER_TEXT**
[`string`](../../data-types.md) | Text of notification for overdue subsequent responses ||
|| **KPI_CHECK_OPERATOR_ACTIVITY**
[`string`](../../data-types.md) | Status of monitoring operator activity. Possible values:
- `Y` — yes
- `N` — no ||
|| **SEND_NOTIFICATION_EMPTY_QUEUE**
[`string`](../../data-types.md) | Status of notifications for an empty queue. Possible values:
- `Y` — yes
- `N` — no ||
|| **USE_WELCOME_FORM**
[`string`](../../data-types.md) | Status of using the welcome form. Possible values:
- `Y` — yes
- `N` — no ||
|| **WELCOME_FORM_ID**
[`string`](../../data-types.md) | Identifier of the welcome form ||
|| **WELCOME_FORM_DELAY**
[`string`](../../data-types.md) | Status of delay in showing the welcome form. Possible values:
- `Y` — yes
- `N` — no ||
|| **SEND_WELCOME_EACH_SESSION**
[`string`](../../data-types.md) | Show welcome message in each session. Possible values:
- `Y` — yes
- `N` — no ||
|| **CONFIRM_CLOSE**
[`string`](../../data-types.md) | Require confirmation for closing. Possible values:
- `Y` — yes
- `N` — no ||
|| **IGNORE_WELCOME_FORM_RESPONSIBLE**
[`string`](../../data-types.md) | Ignore the responsible person in the welcome form. Possible values:
- `Y` — yes
- `N` — no ||
|#

#### QUEUE_USERS_FIELDS Object {#queue-users-fields-item}

#|
|| **Name**
`type` | **Description** ||
|| **USER_NAME**
[`string`](../../data-types.md) | User's name. Possible values:
- `string` — user's name is set
- `null` — user's name is not set ||
|| **USER_WORK_POSITION**
[`string`](../../data-types.md) | User's position. Possible values:
- `string` — position is set
- `null` — position is not set ||
|| **USER_AVATAR**
[`string`](../../data-types.md) | Link to avatar. Possible values:
- `string` — link to avatar is set
- `null` — avatar is not set ||
|| **USER_AVATAR_ID**
[`string`](../../data-types.md) | Identifier of the avatar file ||
|#

#### CONFIG_QUEUE Object {#config-queue-item-fields}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Identifier of the object in the queue ||
|| **ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the object in the queue ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "INVALID_FORMAT",
    "error_description": "A wrong format for the PARAMS field 'filter' is passed"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `INVALID_FORMAT` | A wrong format for the PARAMS field 'select' is passed | The `PARAMS.select` field is not passed as an array ||
|| `400` | `INVALID_FORMAT` | A wrong format for the PARAMS field 'order' is passed | The `PARAMS.order` field is passed in an incorrect format ||
|| `400` | `INVALID_FORMAT` | A wrong format for the PARAMS field 'filter' is passed | The `PARAMS.filter` field is passed in an incorrect format ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-config-add.md)
- [{#T}](./imopenlines-config-update.md)
- [{#T}](./imopenlines-config-get.md)
- [{#T}](./imopenlines-config-delete.md)
- [{#T}](./imopenlines-config-path-get.md)