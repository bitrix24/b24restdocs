# Get Open Line by ID imopenlines.config.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imopenlines.config.get` retrieves the settings of an open line by its ID.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONFIG_ID***
[`integer`](../../data-types.md) | The ID of the open line.

You can obtain the ID of the open line when [creating an open line](./imopenlines-config-add.md) or by using the [method to get the list of open lines](./imopenlines-config-list-get.md) ||
|| **WITH_QUEUE**
[`char`](../../data-types.md) | Return operator queue data. Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_OFFLINE**
[`char`](../../data-types.md) | Return offline operators in the queue. Possible values:
- `Y` — yes
- `N` — no ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CONFIG_ID": 15,
        "WITH_QUEUE": "Y",
        "SHOW_OFFLINE": "Y"
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.config.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CONFIG_ID": 15,
        "WITH_QUEUE": "Y",
        "SHOW_OFFLINE": "Y",
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.config.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.config.get',
            {
                CONFIG_ID: 15,
                WITH_QUEUE: 'Y',
                SHOW_OFFLINE: 'Y'
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
                'imopenlines.config.get',
                [
                    'CONFIG_ID' => 15,
                    'WITH_QUEUE' => 'Y',
                    'SHOW_OFFLINE' => 'Y',
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
        'imopenlines.config.get',
        {
            CONFIG_ID: 15,
            WITH_QUEUE: 'Y',
            SHOW_OFFLINE: 'Y'
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
        'imopenlines.config.get',
        [
            'CONFIG_ID' => 15,
            'WITH_QUEUE' => 'Y',
            'SHOW_OFFLINE' => 'Y',
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": "22",
        "ACTIVE": "Y",
        "LINE_NAME": "Bitrix24 Documentation",
        "CRM": "N",
        "CRM_CREATE": "lead",
        "CRM_CREATE_SECOND": "0",
        "CRM_CREATE_THIRD": "Y",
        "CRM_FORWARD": "Y",
        "CRM_CHAT_TRACKER": "N",
        "CRM_TRANSFER_CHANGE": "Y",
        "CRM_SOURCE": "create",
        "QUEUE_TIME": "60",
        "NO_ANSWER_TIME": "60",
        "QUEUE_TYPE": "evenly",
        "CHECK_AVAILABLE": "Y",
        "WATCH_TYPING": "N",
        "WELCOME_BOT_ENABLE": "Y",
        "WELCOME_MESSAGE": "N",
        "WELCOME_MESSAGE_TEXT": "Welcome to the open line [b]«Bitrix24 Documentation»[/b][br]The first available operator will respond to you.",
        "VOTE_MESSAGE": "Y",
        "VOTE_TIME_LIMIT": "0",
        "VOTE_BEFORE_FINISH": "Y",
        "VOTE_CLOSING_DELAY": "Y",
        "VOTE_MESSAGE_1_TEXT": "Please rate the quality of service.",
        "VOTE_MESSAGE_1_LIKE": "Thank you for your feedback!",
        "VOTE_MESSAGE_1_DISLIKE": "We are sorry we couldn't assist you, we will strive to improve.",
        "VOTE_MESSAGE_2_TEXT": "Please rate the quality of service.\r\n\r\nSend: 1 - good, 0 - bad",
        "VOTE_MESSAGE_2_LIKE": "Thank you for your feedback!",
        "VOTE_MESSAGE_2_DISLIKE": "We are sorry we couldn't assist you, we will strive to improve.",
        "AGREEMENT_MESSAGE": "N",
        "AGREEMENT_ID": "0",
        "CATEGORY_ENABLE": "N",
        "CATEGORY_ID": "0",
        "WELCOME_BOT_JOIN": "always",
        "WELCOME_BOT_ID": "597",
        "WELCOME_BOT_TIME": "180",
        "WELCOME_BOT_LEFT": "close",
        "NO_ANSWER_RULE": "text",
        "NO_ANSWER_FORM_ID": "0",
        "NO_ANSWER_BOT_ID": "0",
        "NO_ANSWER_TEXT": "Unfortunately, we cannot respond to you at this moment, we will definitely get back to you.",
        "WORKTIME_ENABLE": "Y",
        "WORKTIME_FROM": "0",
        "WORKTIME_TO": "23.59",
        "WORKTIME_TIMEZONE": "Europe/Berlin",
        "WORKTIME_HOLIDAYS": [
            ""
        ],
        "WORKTIME_DAYOFF": [
            ""
        ],
        "WORKTIME_DAYOFF_RULE": "text",
        "WORKTIME_DAYOFF_FORM_ID": "0",
        "WORKTIME_DAYOFF_BOT_ID": "0",
        "WORKTIME_DAYOFF_TEXT": "Unfortunately, we cannot respond to you at this moment.[br][br]Please write your question and we will definitely get back to you during working hours.",
        "CLOSE_RULE": "text",
        "CLOSE_FORM_ID": "0",
        "CLOSE_BOT_ID": "0",
        "CLOSE_TEXT": "Thank you for reaching out to us, please rate the quality of service.",
        "FULL_CLOSE_TIME": "0",
        "AUTO_CLOSE_RULE": "none",
        "AUTO_CLOSE_FORM_ID": "0",
        "AUTO_CLOSE_BOT_ID": "0",
        "AUTO_CLOSE_TIME": "3600",
        "AUTO_CLOSE_TEXT": "",
        "AUTO_EXPIRE_TIME": "86400",
        "DATE_CREATE": {},
        "DATE_MODIFY": {},
        "MODIFY_USER_ID": "27",
        "TEMPORARY": "N",
        "XML_ID": null,
        "LANGUAGE_ID": "de",
        "QUICK_ANSWERS_IBLOCK_ID": "181",
        "SESSION_PRIORITY": "0",
        "TYPE_MAX_CHAT": "answered_new",
        "MAX_CHAT": "0",
        "OPERATOR_DATA": "profile",
        "DEFAULT_OPERATOR_DATA": [],
        "KPI_FIRST_ANSWER_TIME": "0",
        "KPI_FIRST_ANSWER_ALERT": "N",
        "KPI_FIRST_ANSWER_LIST": null,
        "KPI_FIRST_ANSWER_TEXT": "Employee #OPERATOR# exceeded the allowable response time to the client for the first message. Dialog №#DIALOG#.",
        "KPI_FURTHER_ANSWER_TIME": "0",
        "KPI_FURTHER_ANSWER_ALERT": "N",
        "KPI_FURTHER_ANSWER_LIST": null,
        "KPI_FURTHER_ANSWER_TEXT": "Employee #OPERATOR# exceeded the allowable response time to the client for the message. Dialog №#DIALOG#.",
        "KPI_CHECK_OPERATOR_ACTIVITY": "N",
        "SEND_NOTIFICATION_EMPTY_QUEUE": "N",
        "USE_WELCOME_FORM": "N",
        "WELCOME_FORM_ID": "111",
        "WELCOME_FORM_DELAY": "Y",
        "SEND_WELCOME_EACH_SESSION": "Y",
        "CONFIRM_CLOSE": "Y",
        "IGNORE_WELCOME_FORM_RESPONSIBLE": "N",
        "QUEUE": [
            "27",
            "103"
        ],
        "QUEUE_FULL": {
            "27": {
                "ID": "245",
                "SORT": "0",
                "USER_ID": "27",
                "DEPARTMENT_ID": "0",
                "USER_NAME": null,
                "USER_WORK_POSITION": null,
                "USER_AVATAR": null,
                "USER_AVATAR_ID": "0"
            },
            "103": {
                "ID": "251",
                "SORT": "1",
                "USER_ID": "103",
                "DEPARTMENT_ID": "0",
                "USER_NAME": null,
                "USER_WORK_POSITION": null,
                "USER_AVATAR": null,
                "USER_AVATAR_ID": "0"
            }
        },
        "QUEUE_USERS_FIELDS": {
            "27": {
                "USER_NAME": null,
                "USER_WORK_POSITION": null,
                "USER_AVATAR": null,
                "USER_AVATAR_ID": "0"
            },
            "103": {
                "USER_NAME": null,
                "USER_WORK_POSITION": null,
                "USER_AVATAR": null,
                "USER_AVATAR_ID": "0"
            }
        },
        "QUEUE_ONLINE": "Y"
    },
    "time": {
        "start": 1773663905,
        "finish": 1773663905.742784,
        "duration": 0.7427840232849121,
        "processing": 0,
        "date_start": "2026-03-16T15:25:05+01:00",
        "date_finish": "2026-03-16T15:25:05+01:00",
        "operating_reset_at": 1773664505,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The result value. Possible values:
- `object` — an object with open line settings [(detailed description)](#result)
- `false` — the line with the specified `CONFIG_ID` does not exist ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | The ID of the open line ||
|| **LINE_NAME**
[`string`](../../data-types.md) | The name of the open line ||
|| **ACTIVE**
[`string`](../../data-types.md) | The status of the line's activity. Possible values:
- `Y` — the line is active
- `N` — the line is inactive ||
|| **CRM**
[`string`](../../data-types.md) | The CRM operation status. Possible values:
- `Y` — yes
- `N` — no ||
|| **CRM_CREATE**
[`string`](../../data-types.md) | The scenario for creating a CRM entity ||
|| **CRM_CREATE_SECOND**
[`string`](../../data-types.md) | Additional mode for creating a CRM entity ||
|| **CRM_CREATE_THIRD**
[`string`](../../data-types.md) | Additional mode for creating a CRM entity. Possible values:
- `Y` — yes
- `N` — no ||
|| **CRM_FORWARD**
[`string`](../../data-types.md) | The status of forwarding to CRM. Possible values:
- `Y` — yes
- `N` — no ||
|| **CRM_CHAT_TRACKER**
[`string`](../../data-types.md) | The status of sending the dialog to the CRM tracker. Possible values:
- `Y` — yes
- `N` — no ||
|| **CRM_TRANSFER_CHANGE**
[`string`](../../data-types.md) | The status of changing the responsible person during transfer. Possible values:
- `Y` — yes
- `N` — no ||
|| **CRM_SOURCE**
[`string`](../../data-types.md) | The mode of working with the source in CRM ||
|| **QUEUE_TYPE**
[`string`](../../data-types.md) | The mode of distributing requests ||
|| **QUEUE_TIME**
[`string`](../../data-types.md) | The time for a request to move to the next operator ||
|| **NO_ANSWER_TIME**
[`string`](../../data-types.md) | The time until the no-answer scenario is triggered ||
|| **CHECK_AVAILABLE**
[`string`](../../data-types.md) | Check the availability of operators. Possible values:
- `Y` — yes
- `N` — no ||
|| **WATCH_TYPING**
[`string`](../../data-types.md) | Show the operator's typing. Possible values:
- `Y` — yes
- `N` — no ||
|| **WELCOME_BOT_ENABLE**
[`string`](../../data-types.md) | The status of using the welcome bot. Possible values:
- `Y` — yes
- `N` — no ||
|| **WELCOME_MESSAGE**
[`string`](../../data-types.md) | The status of the welcome message. Possible values:
- `Y` — yes
- `N` — no ||
|| **WELCOME_MESSAGE_TEXT**
[`string`](../../data-types.md) | The text of the welcome message ||
|| **VOTE_MESSAGE**
[`string`](../../data-types.md) | The status of the feedback request. Possible values:
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
[`string`](../../data-types.md) | The text of the first feedback scenario ||
|| **VOTE_MESSAGE_1_LIKE**
[`string`](../../data-types.md) | The text for positive feedback in the first scenario ||
|| **VOTE_MESSAGE_1_DISLIKE**
[`string`](../../data-types.md) | The text for negative feedback in the first scenario ||
|| **VOTE_MESSAGE_2_TEXT**
[`string`](../../data-types.md) | The text of the second feedback scenario ||
|| **VOTE_MESSAGE_2_LIKE**
[`string`](../../data-types.md) | The text for positive feedback in the second scenario ||
|| **VOTE_MESSAGE_2_DISLIKE**
[`string`](../../data-types.md) | The text for negative feedback in the second scenario ||
|| **AGREEMENT_MESSAGE**
[`string`](../../data-types.md) | The status of displaying the agreement message. Possible values:
- `Y` — yes
- `N` — no ||
|| **AGREEMENT_ID**
[`string`](../../data-types.md) | The ID of the agreement ||
|| **CATEGORY_ENABLE**
[`string`](../../data-types.md) | The status of using categories. Possible values:
- `Y` — yes
- `N` — no ||
|| **CATEGORY_ID**
[`string`](../../data-types.md) | The ID of the category ||
|| **WELCOME_BOT_JOIN**
[`string`](../../data-types.md) | The mode of connecting the welcome bot ||
|| **WELCOME_BOT_ID**
[`string`](../../data-types.md) | The ID of the welcome bot ||
|| **WELCOME_BOT_TIME**
[`string`](../../data-types.md) | The waiting time for connecting the welcome bot ||
|| **WELCOME_BOT_LEFT**
[`string`](../../data-types.md) | The mode of disconnecting the welcome bot ||
|| **NO_ANSWER_RULE**
[`string`](../../data-types.md) | The scenario for no response from the operator ||
|| **NO_ANSWER_FORM_ID**
[`string`](../../data-types.md) | The ID of the no-answer scenario form ||
|| **NO_ANSWER_BOT_ID**
[`string`](../../data-types.md) | The ID of the no-answer scenario bot ||
|| **NO_ANSWER_TEXT**
[`string`](../../data-types.md) | The text of the no-answer message ||
|| **WORKTIME_ENABLE**
[`string`](../../data-types.md) | The status of considering working hours. Possible values:
- `Y` — yes
- `N` — no ||
|| **WORKTIME_FROM**
[`string`](../../data-types.md) | Start of working hours ||
|| **WORKTIME_TO**
[`string`](../../data-types.md) | End of working hours ||
|| **WORKTIME_TIMEZONE**
[`string`](../../data-types.md) | Time zone of working hours ||
|| **WORKTIME_HOLIDAYS**
[`array`](../../data-types.md) | Array of holidays ||
|| **WORKTIME_DAYOFF**
[`array`](../../data-types.md) | Array of days off ||
|| **WORKTIME_DAYOFF_RULE**
[`string`](../../data-types.md) | The scenario for handling requests during non-working hours ||
|| **WORKTIME_DAYOFF_FORM_ID**
[`string`](../../data-types.md) | The ID of the non-working hours scenario form ||
|| **WORKTIME_DAYOFF_BOT_ID**
[`string`](../../data-types.md) | The ID of the non-working hours scenario bot ||
|| **WORKTIME_DAYOFF_TEXT**
[`string`](../../data-types.md) | The text of the message during non-working hours ||
|| **CLOSE_RULE**
[`string`](../../data-types.md) | The scenario for closing the dialog ||
|| **CLOSE_FORM_ID**
[`string`](../../data-types.md) | The ID of the closing form ||
|| **CLOSE_BOT_ID**
[`string`](../../data-types.md) | The ID of the closing bot ||
|| **CLOSE_TEXT**
[`string`](../../data-types.md) | The text of the closing message ||
|| **FULL_CLOSE_TIME**
[`string`](../../data-types.md) | The time for full session closure ||
|| **AUTO_CLOSE_RULE**
[`string`](../../data-types.md) | The scenario for auto-closing ||
|| **AUTO_CLOSE_FORM_ID**
[`string`](../../data-types.md) | The ID of the auto-closing form ||
|| **AUTO_CLOSE_BOT_ID**
[`string`](../../data-types.md) | The ID of the auto-closing bot ||
|| **AUTO_CLOSE_TIME**
[`string`](../../data-types.md) | The time until auto-closing ||
|| **AUTO_CLOSE_TEXT**
[`string`](../../data-types.md) | The text for auto-closing ||
|| **AUTO_EXPIRE_TIME**
[`string`](../../data-types.md) | The time for activity expiration ||
|| **DATE_CREATE**
[`object`](../../data-types.md) | The creation date of the setting in serialized form ||
|| **DATE_MODIFY**
[`object`](../../data-types.md) | The modification date of the setting in serialized form ||
|| **MODIFY_USER_ID**
[`string`](../../data-types.md) | The ID of the user who modified the settings ||
|| **TEMPORARY**
[`string`](../../data-types.md) | The status of a temporary line. Possible values:
- `Y` — temporary line
- `N` — permanent line ||
|| **XML_ID**
[`string`](../../data-types.md) | External ID. Possible values:
- `string` — external ID is set
- `null` — external ID is not set ||
|| **LANGUAGE_ID**
[`string`](../../data-types.md) | The language of the line ||
|| **QUICK_ANSWERS_IBLOCK_ID**
[`string`](../../data-types.md) | The ID of the quick answers information block ||
|| **SESSION_PRIORITY**
[`string`](../../data-types.md) | The session priority ||
|| **TYPE_MAX_CHAT**
[`string`](../../data-types.md) | The mode for limiting active dialogs per operator ||
|| **MAX_CHAT**
[`string`](../../data-types.md) | Maximum active dialogs per operator ||
|| **OPERATOR_DATA**
[`string`](../../data-types.md) | The mode for displaying operator data ||
|| **QUEUE**
[`array`](../../data-types.md) | IDs of operators in the queue ||
|| **QUEUE_FULL**
[`object`](../../data-types.md) | Data of queue items. The key of the object is the user ID, the value is the queue [(detailed description)](#result-queue-full-item) ||
|| **QUEUE_USERS_FIELDS**
[`object`](../../data-types.md) | User fields in the queue. The key of the object is the user ID, the value is the user description [(detailed description)](#result-queue-users-fields-item) ||
|| **QUEUE_ONLINE**
[`string`](../../data-types.md) | The status of checking the online status of operators. Possible values:
- `Y` — yes
- `N` — no ||
|| **DEFAULT_OPERATOR_DATA**
[`array`](../../data-types.md) | Set of default operator display fields ||
|| **KPI_FIRST_ANSWER_TIME**
[`string`](../../data-types.md) | Norm for the first response time ||
|| **KPI_FIRST_ANSWER_ALERT**
[`string`](../../data-types.md) | The status of notification for overdue first response. Possible values:
- `Y` — yes
- `N` — no ||
|| **KPI_FIRST_ANSWER_LIST**
[`array`](../../data-types.md) | List of recipients for first response notifications. Possible values:
- `array` — list of recipients is set
- `null` — list of recipients is not set ||
|| **KPI_FIRST_ANSWER_TEXT**
[`string`](../../data-types.md) | Text for overdue first response notification ||
|| **KPI_FURTHER_ANSWER_TIME**
[`string`](../../data-types.md) | Norm for subsequent response times ||
|| **KPI_FURTHER_ANSWER_ALERT**
[`string`](../../data-types.md) | The status of notification for overdue subsequent responses. Possible values:
- `Y` — yes
- `N` — no ||
|| **KPI_FURTHER_ANSWER_LIST**
[`array`](../../data-types.md) | List of recipients for subsequent response notifications. Possible values:
- `array` — list of recipients is set
- `null` — list of recipients is not set ||
|| **KPI_FURTHER_ANSWER_TEXT**
[`string`](../../data-types.md) | Text for overdue subsequent response notification ||
|| **KPI_CHECK_OPERATOR_ACTIVITY**
[`string`](../../data-types.md) | The status of monitoring operator activity. Possible values:
- `Y` — yes
- `N` — no ||
|| **SEND_NOTIFICATION_EMPTY_QUEUE**
[`string`](../../data-types.md) | The status of notifications for an empty queue. Possible values:
- `Y` — yes
- `N` — no ||
|| **USE_WELCOME_FORM**
[`string`](../../data-types.md) | The status of using the welcome form. Possible values:
- `Y` — yes
- `N` — no ||
|| **WELCOME_FORM_ID**
[`string`](../../data-types.md) | The ID of the welcome form ||
|| **WELCOME_FORM_DELAY**
[`string`](../../data-types.md) | The status of delay in showing the welcome form. Possible values:
- `Y` — yes
- `N` — no ||
|| **SEND_WELCOME_EACH_SESSION**
[`string`](../../data-types.md) | Show the welcome message in each session. Possible values:
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

#### Object QUEUE_FULL {#result-queue-full-item}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | The ID of the queue item record ||
|| **SORT**
[`string`](../../data-types.md) | Position in the queue ||
|| **USER_ID**
[`string`](../../data-types.md) | User ID ||
|| **DEPARTMENT_ID**
[`string`](../../data-types.md) | Department ID ||
|| **USER_NAME**
[`string`](../../data-types.md) | User name. Possible values:
- `string` — user name is set
- `null` — user name is not set ||
|| **USER_WORK_POSITION**
[`string`](../../data-types.md) | User position. Possible values:
- `string` — position is set
- `null` — position is not set ||
|| **USER_AVATAR**
[`string`](../../data-types.md) | Link to avatar. Possible values:
- `string` — link to avatar is set
- `null` — avatar is not set ||
|| **USER_AVATAR_ID**
[`string`](../../data-types.md) | Avatar file ID ||
|#

#### Object QUEUE_USERS_FIELDS {#result-queue-users-fields-item}

#|
|| **Name**
`type` | **Description** ||
|| **USER_NAME**
[`string`](../../data-types.md) | User name. Possible values:
- `string` — user name is set
- `null` — user name is not set ||
|| **USER_WORK_POSITION**
[`string`](../../data-types.md) | User position. Possible values:
- `string` — position is set
- `null` — position is not set ||
|| **USER_AVATAR**
[`string`](../../data-types.md) | Link to avatar. Possible values:
- `string` — link to avatar is set
- `null` — avatar is not set ||
|| **USER_AVATAR_ID**
[`string`](../../data-types.md) | Avatar file ID ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CONFIG_ID_EMPTY",
    "error_description": "Config ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CONFIG_ID_EMPTY` | Config ID can't be empty | The `CONFIG_ID` parameter is not provided or is incorrect ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-config-add.md)
- [{#T}](./imopenlines-config-update.md)
- [{#T}](./imopenlines-config-list-get.md)
- [{#T}](./imopenlines-config-delete.md)
- [{#T}](./imopenlines-config-path-get.md)