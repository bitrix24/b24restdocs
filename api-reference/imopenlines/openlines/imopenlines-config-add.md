# Add Open Channel imopenlines.config.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit open channels

The method `imopenlines.config.add` adds a new open channel.

To allow users to write to the open channel, set the connector settings using the method [imconnector.connector.data.set](../imconnector/imconnector-connector-data-set.md).

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **PARAMS**
[`object`](../../data-types.md) | Object with open channel settings [(detailed description)](#params). If omitted, the channel is created with default settings ||
|#

### PARAMS Parameter {#params}

#|
|| **Name**
`type` | **Description** ||
|| **LINE_NAME**
[`string`](../../data-types.md) | Name of the open channel, up to 255 characters. If omitted, the name is generated automatically ||
|| **ACTIVE**
[`char`](../../data-types.md) | Channel activity. Possible values:
- `Y` — channel is active
- `N` — channel is inactive

Default is `Y` ||
  || **QUEUE**
  [`array`](../../data-types.md) | Queue of operators. An array of objects with fields `ENTITY_TYPE` and `ENTITY_ID`.

Possible values:
- `ENTITY_TYPE` — user `user` or department `department`
- `ENTITY_ID` — identifier of the selected type object. For `user`, specify the `ID` of the user, for `department` — the `ID` of the department

Example:
```js
[
    {
        "ENTITY_TYPE":"user",
        "ENTITY_ID":"1"
    },
    {
        "ENTITY_TYPE":"department",
        "ENTITY_ID":"10"
    }
]
```

If a department is passed, its employees are added to the queue. If the parameter is not passed, the queue remains empty ||
|| **QUEUE_TYPE**
[`string`](../../data-types.md) | Request distribution mode. Possible values:
- `evenly` — distribute requests evenly among operators
- `strictly` — direct requests strictly in queue order
- `all` — direct requests to all operators in the queue

Default is `all`. The method replaces a value not in the list with `all`. If the Bitrix24 plan does not allow the `all` mode, the channel gets `evenly` ||
  || **QUEUE_TIME**
  [`integer`](../../data-types.md) | Time in seconds before the request is passed to the next operator. Default is `60` ||
  || **NO_ANSWER_TIME**
  [`integer`](../../data-types.md) | Time in seconds before the no-answer scenario is triggered. Default is `60` ||
  || **WELCOME_MESSAGE**
  [`char`](../../data-types.md) | Send a welcome message. Possible values:
- `Y` — yes
- `N` — no ||
  || **WELCOME_MESSAGE_TEXT**
  [`string`](../../data-types.md) | Text of the welcome message ||
  || **WORKTIME_ENABLE**
  [`char`](../../data-types.md) | Consider the channel's working hours. Possible values:
- `Y` — yes
- `N` — no

Default is `N`. If the Bitrix24 plan does not allow configuring working hours, the method saves `N` ||
  || **WORKTIME_FROM**
  [`string`](../../data-types.md) | Start of working hours: hours and minutes separated by a dot, for example `9` or `9.30`. The method replaces a value greater than `23.30` with `23.30`.

Pass it together with `WORKTIME_TO`. If either value is in a different format, for example `09:00`, the method saves the time from `9` to `18.30` ||
  || **WORKTIME_TO**
  [`string`](../../data-types.md) | End of working hours in the same format, for example `18` or `18.30`. If the value is not greater than `WORKTIME_FROM`, the method moves the end to one hour after the start ||
  || **WORKTIME_TIMEZONE**
  [`string`](../../data-types.md) | Time zone, for example `Europe/Berlin` ||
  || **CRM**
  [`char`](../../data-types.md) | Search for the client in CRM. Possible values:
- `Y` — yes
- `N` — no ||
  || **CRM_CREATE**
  [`string`](../../data-types.md) | What to create in CRM for a new client. Possible values:
- `lead` — a lead
- `deal` — a deal
- `none` — nothing

Default is `lead`. The method replaces a value not in the list with `lead` ||
  || **VOTE_MESSAGE**
  [`char`](../../data-types.md) | Request service quality rating. Possible values:
- `Y` — yes
- `N` — no ||
  || **LANGUAGE_ID**
  [`string`](../../data-types.md) | Language of the channel, for example `de`. The method saves the first two characters ||
  || **CRM_CREATE_SECOND**
  [`string`](../../data-types.md) | Identifier of the pipeline in which the deal is created if `CRM_CREATE` = `deal`. You can retrieve pipeline identifiers using the [crm.category.list](../../crm/universal/category/crm-category-list.md) method with `entityTypeId` = `2`. If not passed, the deal is created in the general pipeline ||
  || **CRM_CREATE_THIRD**
  [`char`](../../data-types.md) | Rule for deals if `CRM_CREATE` = `deal`. Possible values:
- `Y` — the default rule
- `N` — create a new deal for each request

Default is `Y` ||
  || **CRM_CHAT_TRACKER**
  [`char`](../../data-types.md) | Enable chat tracker for CRM. Possible values:
- `Y` — yes
- `N` — no ||
  || **CRM_FORWARD**
  [`char`](../../data-types.md) | Forward the request to the responsible person in CRM. Possible values:
- `Y` — yes
- `N` — no ||
  || **CRM_TRANSFER_CHANGE**
  [`char`](../../data-types.md) | Change the responsible person in CRM when transferring the dialogue. Possible values:
- `Y` — yes
- `N` — no ||
  || **CRM_SOURCE**
  [`string`](../../data-types.md) | Source for the created CRM object. Possible values:
- `create` — create a separate source for the channel for each communication channel
- the `STATUS_ID` of a source from the CRM directory. You can retrieve the list of sources using the [crm.status.list](../../crm/status/crm-status-list.md) method with the filter `ENTITY_ID` = `SOURCE`

Default is `create`. If there is no source with the passed identifier, the channel works as with `create` ||
  || **MAX_CHAT**
  [`integer`](../../data-types.md) | Maximum number of simultaneous dialogues for the operator. `0` means no limit. Default is `0` ||
  || **TYPE_MAX_CHAT**
  [`string`](../../data-types.md) | Type of `MAX_CHAT` restriction. Possible values:
- `answered` — consider only dialogues where the operator has already responded
- `answered_new` — consider dialogues with a response and new dialogues
- `closed` — consider closed dialogues

Default is `answered`. The method replaces a value not in the list with `answered_new` ||
  || **CATEGORY_ENABLE**
  [`char`](../../data-types.md) | Enable categories. Possible values:
- `Y` — yes
- `N` — no ||
  || **CATEGORY_ID**
  [`integer`](../../data-types.md) | Identifier of the category. The method saves the value only if `CATEGORY_ENABLE` = `Y` ||
  || **SESSION_PRIORITY**
  [`integer`](../../data-types.md) | Session priority in the queue, from `0` to `86400`. Default is `0` ||
  || **WELCOME_BOT_ENABLE**
  [`char`](../../data-types.md) | Enable welcome chatbot. Possible values:
- `Y` — yes
- `N` — no ||
  || **WELCOME_BOT_JOIN**
  [`string`](../../data-types.md) | When to connect the bot. Possible values:
- `first` — connect the bot only at the beginning of the dialogue
- `always` — connect the bot in every dialogue ||
  || **WELCOME_BOT_ID**
  [`integer`](../../data-types.md) | Identifier of the chatbot. The method saves the value only if `WELCOME_BOT_ENABLE` = `Y` ||
  || **WELCOME_BOT_TIME**
  [`integer`](../../data-types.md) | After how many seconds to transfer the dialogue from the bot to the queue. Default is `600` ||
  || **WELCOME_BOT_LEFT**
  [`string`](../../data-types.md) | When to disconnect the bot. Possible values:
- `queue` — disconnect the bot when transferring to the operators' queue
- `close` — disconnect the bot when closing the dialogue ||
  || **CHECK_AVAILABLE**
  [`char`](../../data-types.md) | Check the availability of the operator during distribution. Possible values:
- `Y` — yes
- `N` — no ||
  || **WATCH_TYPING**
  [`char`](../../data-types.md) | Show typing indicator. Possible values:
- `Y` — yes
- `N` — no ||
  || **SHOW_NOTIFICATION_REDIRECT**
  [`char`](../../data-types.md) | Show a notification in the dialog when a request passes to an operator who has started their working day. Possible values: `Y` or `N`. By default, the value is not set and the notification is not shown ||
  || **SEND_WELCOME_EACH_SESSION**
  [`char`](../../data-types.md) | Send a greeting in each session. Possible values:
- `Y` — yes
- `N` — no ||
  || **OPERATOR_DATA**
  [`string`](../../data-types.md) | Format of operator data. Possible values:
- `profile` — show operator profile
- `queue` — show operator data from the queue
- `hide` — hide operator data ||
  || **DEFAULT_OPERATOR_DATA**
  [`object`](../../data-types.md) | Default operator data. Possible fields:
- `NAME` — operator's name
- `AVATAR` — link to the operator's avatar
- `AVATAR_ID` — identifier of the avatar file ||
  || **NO_ANSWER_RULE**
  [`string`](../../data-types.md) | Scenario if the operator did not respond. Possible values:
- `none` — do not execute the scenario
- `text` — send a text message ||
  || **NO_ANSWER_FORM_ID**
  [`integer`](../../data-types.md) | Identifier of the CRM form for the no-answer scenario ||
  || **NO_ANSWER_BOT_ID**
  [`integer`](../../data-types.md) | Identifier of the chatbot for the no-answer scenario ||
  || **NO_ANSWER_TEXT**
  [`string`](../../data-types.md) | Text for the no-answer scenario ||
  || **WORKTIME_DAYOFF**
  [`array`](../../data-types.md) | Days off. The method skips values not in the list. Possible values of elements:
- `MO` — Monday
- `TU` — Tuesday
- `WE` — Wednesday
- `TH` — Thursday
- `FR` — Friday
- `SA` — Saturday
- `SU` — Sunday ||
  || **WORKTIME_HOLIDAYS**
  [`string`](../../data-types.md) \| [`array`](../../data-types.md) | Holidays in the `D.MM` format: a comma-separated string, for example `1.01,7.01`, or an array of strings. If the format is invalid, the method saves an empty list ||
  || **WORKTIME_DAYOFF_RULE**
  [`string`](../../data-types.md) | Scenario during non-working hours. Possible values:
- `none` — do not execute the scenario
- `text` — send a text message ||
  || **WORKTIME_DAYOFF_FORM_ID**
  [`integer`](../../data-types.md) | Identifier of the CRM form for non-working hours ||
  || **WORKTIME_DAYOFF_BOT_ID**
  [`integer`](../../data-types.md) | Identifier of the chatbot for non-working hours ||
  || **WORKTIME_DAYOFF_TEXT**
  [`string`](../../data-types.md) | Text message during non-working hours ||
  || **CLOSE_RULE**
  [`string`](../../data-types.md) | Scenario upon closure. Possible values:
- `none` — do not execute the scenario
- `text` — send a text message

Default is `text` ||
  || **CLOSE_FORM_ID**
  [`integer`](../../data-types.md) | Identifier of the CRM form upon closure ||
  || **CLOSE_BOT_ID**
  [`integer`](../../data-types.md) | Identifier of the chatbot upon closure ||
  || **CLOSE_TEXT**
  [`string`](../../data-types.md) | Text message upon closure ||
  || **VOTE_TIME_LIMIT**
  [`integer`](../../data-types.md) | Time limit for voting. `0` means no limit ||
  || **VOTE_CLOSING_DELAY**
  [`char`](../../data-types.md) | Close the session immediately after rating. Possible values:
- `Y` — yes
- `N` — no ||
  || **VOTE_BEFORE_FINISH**
  [`char`](../../data-types.md) | Request rating before completion. Possible values:
- `Y` — yes
- `N` — no ||
  || **VOTE_MESSAGE_1_TEXT**
  [`string`](../../data-types.md) | Text for rating request in the widget, up to 100 characters ||
  || **VOTE_MESSAGE_1_LIKE**
  [`string`](../../data-types.md) | Text after positive rating in the widget, up to 100 characters ||
  || **VOTE_MESSAGE_1_DISLIKE**
  [`string`](../../data-types.md) | Text after negative rating in the widget, up to 100 characters ||
  || **VOTE_MESSAGE_2_TEXT**
  [`string`](../../data-types.md) | Text for rating request in external channels ||
  || **VOTE_MESSAGE_2_LIKE**
  [`string`](../../data-types.md) | Text after positive rating in external channels ||
  || **VOTE_MESSAGE_2_DISLIKE**
  [`string`](../../data-types.md) | Text after negative rating in external channels ||
  || **AUTO_CLOSE_RULE**
  [`string`](../../data-types.md) | Auto-closing scenario. Possible values:
- `none` — do not execute the scenario
- `text` — send a text message ||
  || **FULL_CLOSE_TIME**
  [`integer`](../../data-types.md) | Time in minutes before the dialogue is fully closed after manual closure. Default is `10` ||
  || **AUTO_CLOSE_TIME**
  [`integer`](../../data-types.md) | Time in seconds before the dialogue is auto-closed. Default is `14400` ||
  || **AUTO_CLOSE_FORM_ID**
  [`integer`](../../data-types.md) | Identifier of the CRM form for auto-closure ||
  || **AUTO_CLOSE_BOT_ID**
  [`integer`](../../data-types.md) | Identifier of the chatbot for auto-closure ||
  || **AUTO_CLOSE_TEXT**
  [`string`](../../data-types.md) | Text upon auto-closure ||
  || **AUTO_EXPIRE_TIME**
  [`integer`](../../data-types.md) | Lifetime of an automatically closed session in seconds. Default is `86400` ||
  || **TEMPORARY**
  [`char`](../../data-types.md) | Create a temporary channel. Possible values:
- `Y` — create a temporary channel. Bitrix24 deletes it after 8 hours
- `N` — create a permanent channel

Default is `N` ||
  || **QUICK_ANSWERS_IBLOCK_ID**
  [`integer`](../../data-types.md) | Identifier of the quick answers info block ||
  || **KPI_FIRST_ANSWER_TIME**
  [`integer`](../../data-types.md) | Control of the first response time ||
  || **KPI_FIRST_ANSWER_ALERT**
  [`char`](../../data-types.md) | Enable notification for the first response. Possible values:
- `Y` — yes
- `N` — no ||
  || **KPI_FIRST_ANSWER_LIST**
  [`array`](../../data-types.md) | List of recipients for the first response notification ||
  || **KPI_FIRST_ANSWER_TEXT**
  [`string`](../../data-types.md) | Text of the first response notification ||
  || **KPI_FURTHER_ANSWER_TIME**
  [`integer`](../../data-types.md) | Control of subsequent response times ||
  || **KPI_FURTHER_ANSWER_ALERT**
  [`char`](../../data-types.md) | Enable notification for subsequent responses. Possible values:
- `Y` — yes
- `N` — no ||
  || **KPI_FURTHER_ANSWER_LIST**
  [`array`](../../data-types.md) | List of recipients for subsequent response notifications ||
  || **KPI_FURTHER_ANSWER_TEXT**
  [`string`](../../data-types.md) | Text of the subsequent response notification ||
  || **KPI_CHECK_OPERATOR_ACTIVITY**
  [`char`](../../data-types.md) | Monitor operator activity. Possible values:
- `Y` — yes
- `N` — no ||
  || **USE_WELCOME_FORM**
  [`char`](../../data-types.md) | Use welcome CRM form. Possible values:
- `Y` — yes
- `N` — no ||
  || **WELCOME_FORM_ID**
  [`integer`](../../data-types.md) | Identifier of the welcome CRM form ||
  || **WELCOME_FORM_DELAY**
  [`char`](../../data-types.md) | Show the form with a delay. Possible values:
- `Y` — yes
- `N` — no ||
  || **AGREEMENT_MESSAGE**
  [`char`](../../data-types.md) | Show a warning about personal data collection. Possible values:
- `Y` — yes
- `N` — no

Default is `N`. If you pass `Y`, specify the agreement in `AGREEMENT_ID`, otherwise the method returns `false` ||
  || **AGREEMENT_ID**
  [`integer`](../../data-types.md) | Identifier of the personal data processing agreement ||
  || **CONFIRM_CLOSE**
  [`char`](../../data-types.md) | Request confirmation for closure. Possible values:
- `Y` — yes
- `N` — no ||
  || **IGNORE_WELCOME_FORM_RESPONSIBLE**
  [`char`](../../data-types.md) | Ignore the responsible person in the welcome form. Possible values:
- `Y` — yes
- `N` — no ||
  |#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PARAMS": {
          "LINE_NAME": "Online Store Support Line",
          "QUEUE": [
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "1"
            },
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "15"
            },
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "23"
            }
          ],
          "QUEUE_TYPE": "strictly",
          "QUEUE_TIME": 45,
          "NO_ANSWER_TIME": 120,
          "WELCOME_MESSAGE": "Y",
          "WELCOME_MESSAGE_TEXT": "Hello! We will respond within a couple of minutes",
          "CRM": "Y",
          "CRM_CREATE": "deal",
          "CRM_SOURCE": "create",
          "CRM_FORWARD": "Y",
          "MAX_CHAT": 4,
          "TYPE_MAX_CHAT": "answered_new",
          "WORKTIME_ENABLE": "Y",
          "WORKTIME_FROM": "9",
          "WORKTIME_TO": "21",
          "WORKTIME_TIMEZONE": "Europe/Berlin",
          "WORKTIME_DAYOFF": [
            "SA",
            "SU"
          ],
          "WORKTIME_DAYOFF_RULE": "text",
          "WORKTIME_DAYOFF_TEXT": "The line is currently closed. Please write, and we will respond during working hours"
        }
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.config.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PARAMS": {
          "LINE_NAME": "Online Store Support Line",
          "QUEUE": [
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "1"
            },
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "15"
            },
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "23"
            }
          ],
          "QUEUE_TYPE": "strictly",
          "QUEUE_TIME": 45,
          "NO_ANSWER_TIME": 120,
          "WELCOME_MESSAGE": "Y",
          "WELCOME_MESSAGE_TEXT": "Hello! We will respond within a couple of minutes",
          "CRM": "Y",
          "CRM_CREATE": "deal",
          "CRM_SOURCE": "create",
          "CRM_FORWARD": "Y",
          "MAX_CHAT": 4,
          "TYPE_MAX_CHAT": "answered_new",
          "WORKTIME_ENABLE": "Y",
          "WORKTIME_FROM": "9",
          "WORKTIME_TO": "21",
          "WORKTIME_TIMEZONE": "Europe/Berlin",
          "WORKTIME_DAYOFF": [
            "SA",
            "SU"
          ],
          "WORKTIME_DAYOFF_RULE": "text",
          "WORKTIME_DAYOFF_TEXT": "The line is currently closed. Please write, and we will respond during working hours"
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.config.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'imopenlines.config.add',
        params: {
          PARAMS: {
            LINE_NAME: 'Online Store Support Line',
            QUEUE: [
              { ENTITY_TYPE: 'user', ENTITY_ID: '1' },
              { ENTITY_TYPE: 'user', ENTITY_ID: '15' },
              { ENTITY_TYPE: 'user', ENTITY_ID: '23' },
            ],
            QUEUE_TYPE: 'strictly',
            QUEUE_TIME: 45,
            NO_ANSWER_TIME: 120,
            WELCOME_MESSAGE: 'Y',
            WELCOME_MESSAGE_TEXT: 'Hello! We will respond within a couple of minutes',
            CRM: 'Y',
            CRM_CREATE: 'deal',
            CRM_SOURCE: 'create',
            CRM_FORWARD: 'Y',
            MAX_CHAT: 4,
            TYPE_MAX_CHAT: 'answered_new',
            WORKTIME_ENABLE: 'Y',
            WORKTIME_FROM: '9',
            WORKTIME_TO: '21',
            WORKTIME_TIMEZONE: 'Europe/Berlin',
            WORKTIME_DAYOFF: ['SA', 'SU'],
            WORKTIME_DAYOFF_RULE: 'text',
            WORKTIME_DAYOFF_TEXT: 'The line is currently closed. Please write, and we will respond during working hours',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created open line ID:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addOpenlinesConfig() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.config.add',
            params: {
              PARAMS: {
                LINE_NAME: 'Online Store Support Line',
                QUEUE: [
                  { ENTITY_TYPE: 'user', ENTITY_ID: '1' },
                  { ENTITY_TYPE: 'user', ENTITY_ID: '15' },
                  { ENTITY_TYPE: 'user', ENTITY_ID: '23' },
                ],
                QUEUE_TYPE: 'strictly',
                QUEUE_TIME: 45,
                NO_ANSWER_TIME: 120,
                WELCOME_MESSAGE: 'Y',
                WELCOME_MESSAGE_TEXT: 'Hello! We will respond within a couple of minutes',
                CRM: 'Y',
                CRM_CREATE: 'deal',
                CRM_SOURCE: 'create',
                CRM_FORWARD: 'Y',
                MAX_CHAT: 4,
                TYPE_MAX_CHAT: 'answered_new',
                WORKTIME_ENABLE: 'Y',
                WORKTIME_FROM: '9',
                WORKTIME_TO: '21',
                WORKTIME_TIMEZONE: 'Europe/Berlin',
                WORKTIME_DAYOFF: ['SA', 'SU'],
                WORKTIME_DAYOFF_RULE: 'text',
                WORKTIME_DAYOFF_TEXT: 'The line is currently closed. Please write, and we will respond during working hours',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created open line ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addOpenlinesConfig)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imopenlines.config.add(
            params={
                "LINE_NAME": "Online Store Support Line",
                "QUEUE": [
                    {
                        "ENTITY_TYPE": "user",
                        "ENTITY_ID": "1",
                    },
                    {
                        "ENTITY_TYPE": "user",
                        "ENTITY_ID": "15",
                    },
                    {
                        "ENTITY_TYPE": "user",
                        "ENTITY_ID": "23",
                    },
                ],
                "QUEUE_TYPE": "strictly",
                "QUEUE_TIME": 45,
                "NO_ANSWER_TIME": 120,
                "WELCOME_MESSAGE": "Y",
                "WELCOME_MESSAGE_TEXT": "Hello! We will respond within a couple of minutes",
                "CRM": "Y",
                "CRM_CREATE": "deal",
                "CRM_SOURCE": "create",
                "CRM_FORWARD": "Y",
                "MAX_CHAT": 4,
                "TYPE_MAX_CHAT": "answered_new",
                "WORKTIME_ENABLE": "Y",
                "WORKTIME_FROM": "9",
                "WORKTIME_TO": "21",
                "WORKTIME_TIMEZONE": "Europe/Berlin",
                "WORKTIME_DAYOFF": [
                    "SA",
                    "SU",
                ],
                "WORKTIME_DAYOFF_RULE": "text",
                "WORKTIME_DAYOFF_TEXT": "The line is currently closed. Please write, and we will respond during working hours",
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.config.add',
                [
                    'PARAMS' => [
                        'LINE_NAME' => 'Online Store Support Line',
                        'QUEUE' => [
                            [
                                'ENTITY_TYPE' => 'user',
                                'ENTITY_ID' => '1',
                            ],
                            [
                                'ENTITY_TYPE' => 'user',
                                'ENTITY_ID' => '15',
                            ],
                            [
                                'ENTITY_TYPE' => 'user',
                                'ENTITY_ID' => '23',
                            ],
                        ],
                        'QUEUE_TYPE' => 'strictly',
                        'QUEUE_TIME' => 45,
                        'NO_ANSWER_TIME' => 120,
                        'WELCOME_MESSAGE' => 'Y',
                        'WELCOME_MESSAGE_TEXT' => 'Hello! We will respond within a couple of minutes',
                        'CRM' => 'Y',
                        'CRM_CREATE' => 'deal',
                        'CRM_SOURCE' => 'create',
                        'CRM_FORWARD' => 'Y',
                        'MAX_CHAT' => 4,
                        'TYPE_MAX_CHAT' => 'answered_new',
                        'WORKTIME_ENABLE' => 'Y',
                        'WORKTIME_FROM' => '9',
                        'WORKTIME_TO' => '21',
                        'WORKTIME_TIMEZONE' => 'Europe/Berlin',
                        'WORKTIME_DAYOFF' => ['SA', 'SU'],
                        'WORKTIME_DAYOFF_RULE' => 'text',
                        'WORKTIME_DAYOFF_TEXT' => 'The line is currently closed. Please write, and we will respond during working hours',
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
        'imopenlines.config.add',
        {
            PARAMS: {
                LINE_NAME: 'Online Store Support Line',
                QUEUE: [
                    {
                        ENTITY_TYPE: 'user',
                        ENTITY_ID: '1'
                    },
                    {
                        ENTITY_TYPE: 'user',
                        ENTITY_ID: '15'
                    },
                    {
                        ENTITY_TYPE: 'user',
                        ENTITY_ID: '23'
                    }
                ],
                QUEUE_TYPE: 'strictly',
                QUEUE_TIME: 45,
                NO_ANSWER_TIME: 120,
                WELCOME_MESSAGE: 'Y',
                WELCOME_MESSAGE_TEXT: 'Hello! We will respond within a couple of minutes',
                CRM: 'Y',
                CRM_CREATE: 'deal',
                CRM_SOURCE: 'create',
                CRM_FORWARD: 'Y',
                MAX_CHAT: 4,
                TYPE_MAX_CHAT: 'answered_new',
                WORKTIME_ENABLE: 'Y',
                WORKTIME_FROM: '9',
                WORKTIME_TO: '21',
                WORKTIME_TIMEZONE: 'Europe/Berlin',
                WORKTIME_DAYOFF: ['SA', 'SU'],
                WORKTIME_DAYOFF_RULE: 'text',
                WORKTIME_DAYOFF_TEXT: 'The line is currently closed. Please write, and we will respond during working hours'
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
        'imopenlines.config.add',
        [
            'PARAMS' => [
                'LINE_NAME' => 'Online Store Support Line',
                'QUEUE' => [
                    [
                        'ENTITY_TYPE' => 'user',
                        'ENTITY_ID' => '1',
                    ],
                    [
                        'ENTITY_TYPE' => 'user',
                        'ENTITY_ID' => '15',
                    ],
                    [
                        'ENTITY_TYPE' => 'user',
                        'ENTITY_ID' => '23',
                    ],
                ],
                'QUEUE_TYPE' => 'strictly',
                'QUEUE_TIME' => 45,
                'NO_ANSWER_TIME' => 120,
                'WELCOME_MESSAGE' => 'Y',
                'WELCOME_MESSAGE_TEXT' => 'Hello! We will respond within a couple of minutes',
                'CRM' => 'Y',
                'CRM_CREATE' => 'deal',
                'CRM_SOURCE' => 'create',
                'CRM_FORWARD' => 'Y',
                'MAX_CHAT' => 4,
                'TYPE_MAX_CHAT' => 'answered_new',
                'WORKTIME_ENABLE' => 'Y',
                'WORKTIME_FROM' => '9',
                'WORKTIME_TO' => '21',
                'WORKTIME_TIMEZONE' => 'Europe/Berlin',
                'WORKTIME_DAYOFF' => ['SA', 'SU'],
                'WORKTIME_DAYOFF_RULE' => 'text',
                'WORKTIME_DAYOFF_TEXT' => 'The line is currently closed. Please write, and we will respond during working hours',
            ],
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imopenlines.config.add", b24.Params{
    	"PARAMS": b24.Params{
    		"LINE_NAME": "Online Store Support Line",
    		"QUEUE": []b24.Params{
    			{
    				"ENTITY_TYPE": "user",
    				"ENTITY_ID":   "1",
    			},
    			{
    				"ENTITY_TYPE": "user",
    				"ENTITY_ID":   "15",
    			},
    			{
    				"ENTITY_TYPE": "user",
    				"ENTITY_ID":   "23",
    			},
    		},
    		"QUEUE_TYPE":           "strictly",
    		"QUEUE_TIME":           45,
    		"NO_ANSWER_TIME":       120,
    		"WELCOME_MESSAGE":      "Y",
    		"WELCOME_MESSAGE_TEXT": "Hello! We will respond within a couple of minutes",
    		"CRM":                  "Y",
    		"CRM_CREATE":           "deal",
    		"CRM_SOURCE":           "create",
    		"CRM_FORWARD":          "Y",
    		"MAX_CHAT":             4,
    		"TYPE_MAX_CHAT":        "answered_new",
    		"WORKTIME_ENABLE":      "Y",
    		"WORKTIME_FROM":        "9",
    		"WORKTIME_TO":          "21",
    		"WORKTIME_TIMEZONE":    "Europe/Berlin",
    		"WORKTIME_DAYOFF":      []string{"SA", "SU"},
    		"WORKTIME_DAYOFF_RULE": "text",
    		"WORKTIME_DAYOFF_TEXT": "The line is currently closed. Please write, and we will respond during working hours",
    	},
    })
    if err != nil {
    	return fmt.Errorf("imopenlines.config.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 15,
    "time": {
        "start": 1741688359.737944,
        "finish": 1741688360.028349,
        "duration": 0.2904050350189209,
        "processing": 0.16270899772644043,
        "date_start": "2025-03-11T10:25:59+01:00",
        "date_finish": "2025-03-11T10:26:00+01:00",
        "operating_reset_at": 1741688960,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) \| [`boolean`](../../data-types.md) | Identifier of the created open channel.

The method returns `false` if the channel could not be saved, for example, with `AGREEMENT_MESSAGE` = `Y` without an available agreement in `AGREEMENT_ID` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "CONFIG_WRONG_USER_PERMISSION",
    "error_description": "Permission denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `CONFIG_WRONG_USER_PERMISSION` | Permission denied | Insufficient permissions to create an open channel ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-config-update.md)
- [{#T}](./imopenlines-config-get.md)
- [{#T}](./imopenlines-config-list-get.md)
- [{#T}](./imopenlines-config-delete.md)
- [{#T}](./imopenlines-config-path-get.md)
