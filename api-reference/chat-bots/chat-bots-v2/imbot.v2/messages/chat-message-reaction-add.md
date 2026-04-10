# Add Reaction to Message imbot.v2.Chat.Message.Reaction.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.Message.Reaction.add` adds a bot reaction to a message.

## Method Parameters

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **messageId***
[`integer`](../../../../data-types.md) | Message ID ||
|| **reaction***
[`string`](../../../../data-types.md) | Reaction code. The list of available codes is described [below](#reactions) ||
|#

### Available Reaction Codes {#reactions}

#|
|| **Code** | **Description** ||
|| `like` | Like ||
|| `dislike` | Dislike ||
|| `faceWithTearsOfJoy` | Laughing to tears ||
|| `redHeart` | Heart ||
|| `neutralFace` | Indifferent ||
|| `fire` | Fire! ||
|| `cry` | Sad ||
|| `slightlySmilingFace` | Smiling ||
|| `winkingFace` | Winking ||
|| `laugh` | Laughing ||
|| `kiss` | Admiring ||
|| `wonder` | Shocked ||
|| `slightlyFrowningFace` | Frowning ||
|| `loudlyCryingFace` | Crying loudly ||
|| `faceWithStuckOutTongue` | Tongue out ||
|| `faceWithStuckOutTongueAndWinkingEye` | Teasing ||
|| `smilingFaceWithSunglasses` | Cool ||
|| `confusedFace` | Not sure ||
|| `flushedFace` | Embarrassed ||
|| `thinkingFace` | Doubting ||
|| `angry` | Angry ||
|| `smilingFaceWithHorns` | Smirking ||
|| `faceWithThermometer` | Sick ||
|| `facepalm` | No comment ||
|| `poo` | Yuck ||
|| `flexedBiceps` | Strong ||
|| `clappingHands` | Awesome ||
|| `raisedHand` | High five ||
|| `smilingFaceWithHeartEyes` | Beautiful ||
|| `smilingFaceWithHearts` | Love it ||
|| `pleadingFace` | Please ||
|| `relievedFace` | Zen ||
|| `foldedHands` | Thank you ||
|| `okHand` | OK ||
|| `signHorns` | Rock! ||
|| `loveYouGesture` | All good ||
|| `clownFace` | Clown ||
|| `partyingFace` | Congratulations ||
|| `questionMark` | Question ||
|| `exclamationMark` | Attention ||
|| `lightBulb` | Idea ||
|| `bomb` | Bomb ||
|| `sleepingSymbol` | Sleeping ||
|| `crossMark` | Cancel ||
|| `whiteHeavyCheckMark` | Done ||
|| `eyes` | Eyes ||
|| `handshake` | Deal ||
|| `hundredPoints` | Support ||
|#

The list of reactions may expand or contract without prior notice.

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","messageId":789,"reaction":"like"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Message.Reaction.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"messageId":789,"reaction":"like","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Message.Reaction.add
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.Message.Reaction.add', {
        botId: 456,
        messageId: 789,
        reaction: 'like',
      });

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.v2.Chat.Message.Reaction.add',
                [
                    'botId' => 456,
                    'messageId' => 789,
                    'reaction' => 'like',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: ' . print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.Message.Reaction.add',
        {
            botId: 456,
            messageId: 789,
            reaction: 'like',
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.v2.Chat.Message.Reaction.add',
        [
            'botId' => 456,
            'messageId' => 789,
            'reaction' => 'like',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Reaction added';
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": true,
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | `true` if the reaction was successfully added ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "REACTION_NOT_FOUND",
    "error_description": "Reaction not found"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is not provided. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` is not provided ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot is registered by another application ||
|| `ACCESS_DENIED` | Access denied | Bot is not a participant in the chat with this message ||
|| `REACTION_NOT_FOUND` | Reaction not found | Non-existent reaction code specified ||
|| `REACTION_ALREADY_SET` | Reaction already set | This reaction is already set by the bot on this message ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./chat-message-reaction-delete.md)
- [{#T}](./chat-message-send.md)