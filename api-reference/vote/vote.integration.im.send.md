# Create and Send a Vote in Chat vote.Integration.Im.send

> Scope: [`vote`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `vote.Integration.Im.send` creates and sends a vote to the specified chat in the messenger.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **chatId***
[`integer`](../data-types.md)| The identifier of the chat to which the vote is sent. It can be obtained using the methods [im.chat.add](../chats/im-chat-add.md), [im.chat.get](../chats/im-chat-get.md), [im.recent.get](../chats/im-recent-get.md), [im.recent.list](../chats/im-recent-list.md) ||
|| **IM_MESSAGE_VOTE_DATA***
[`object`](../data-types.md)| Vote data containing the question and answer options. The structure is described [below](#MESSAGE) ||
|| **templateId**
[`string`](../data-types.md)| A unique identifier for the request, with no format requirements. The purpose of the identifier is to prevent duplication. If the request is sent again due to a network failure with the same `templateId`, the server will recognize this and create the vote only once ||
|#

### Parameter IM_MESSAGE_VOTE_DATA {#MESSAGE}

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **QUESTIONS***
[`array`](../data-types.md)| An array of vote questions, the structure is described [below](#QUESTIONS). Maximum 1 question ||
|| **ANONYMITY**
[`integer`](../data-types.md)| Anonymity of the vote. Possible values:
- `0` — non-anonymous vote,
- `1` — anonymous vote.

Default value: `0`, public vote ||
|| **OPTIONS**
[`integer`](../data-types.md)| Allow re-voting. Possible values:
- `0` — no,
- `1` — yes.
  
Default value: `0`, re-voting is prohibited ||
|#

#### Structure of the Question in QUESTIONS {#QUESTIONS}

#|
|| **Name**
`type` | **Description** ||
|| **QUESTION**
[`string`](../data-types.md)| The text of the question ||
|| **FIELD_TYPE**
[`integer`](../data-types.md)| The type of field for the answer. Possible values:
- `0` — radio buttons, one answer option,
- `1` — checkboxes, multiple choice.

Default value: `0` ||
|| **ANSWERS**
[`array`](../data-types.md)| An array of answer options, the structure is described [below](#ANSWERS). Minimum 2 options, maximum 10 ||
|#

##### Structure of the Answer in ANSWERS {#ANSWERS}

#|
|| **Name**
`type` | **Description** ||
|| **MESSAGE**
[`string`](../data-types.md)| The text of the answer option ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"chatId":**put_chat_id**,"IM_MESSAGE_VOTE_DATA":{"QUESTIONS":[{"QUESTION":"**put_question_title**","FIELD_TYPE":0,"ANSWERS":[{"MESSAGE":"**put_message_content**"},{"MESSAGE":"**put_message_content**"},{"MESSAGE":"**put_message_content**"}]}],"ANONYMITY":0,"OPTIONS":0},"templateId":null}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/vote.Integration.Im.send
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"chatId":**put_chat_id**,"IM_MESSAGE_VOTE_DATA":{"QUESTIONS":[{"QUESTION":"**put_question_title**","FIELD_TYPE":0,"ANSWERS":[{"MESSAGE":"**put_message_content**"},{"MESSAGE":"**put_message_content**"},{"MESSAGE":"**put_message_content**"}]}],"ANONYMITY":0,"OPTIONS":0},"templateId":null,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/vote.Integration.Im.send
    ```

- JS

    ```js  
    try
    {
        const response = await $b24.callMethod(
            'vote.Integration.Im.send',
            {
                chatId: **put_chat_id**,
                IM_MESSAGE_VOTE_DATA: {
                    QUESTIONS: [
                        {
                            QUESTION: '**put_question_title**',
                            FIELD_TYPE: 0,
                            ANSWERS: [
                                { MESSAGE: '**put_message_content**' },
                                { MESSAGE: '**put_message_content**' },
                                { MESSAGE: '**put_message_content**' }
                            ]
                        }
                    ],
                    ANONYMITY: 0,
                    OPTIONS: 0
                },
                templateId: null
            }
        );

        const result = response.getData().result;
        console.log('Created element with ID:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php  
    try {
        $response = $b24Service
            ->core
            ->call(
                'vote.Integration.Im.send',
                [
                    'chatId' => **put_chat_id**,
                    'IM_MESSAGE_VOTE_DATA' => [
                        'QUESTIONS' => [
                            [
                                'QUESTION' => '**put_question_title**',
                                'FIELD_TYPE' => 0,
                                'ANSWERS' => [
                                    ['MESSAGE' => '**put_message_content**'],
                                    ['MESSAGE' => '**put_message_content**'],
                                    ['MESSAGE' => '**put_message_content**']
                                ]
                            ]
                        ],
                        'ANONYMITY' => 0,
                        'OPTIONS' => 0
                    ],
                    'templateId' => null
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sending vote message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "vote.Integration.Im.send",
        {
            "chatId": **put_chat_id**,
            "IM_MESSAGE_VOTE_DATA": {
                "QUESTIONS": [
                    {
                        "QUESTION": "**put_question_title**",
                        "FIELD_TYPE": 0,
                        "ANSWERS": [
                            {
                                "MESSAGE": "**put_message_content**"
                            },
                            {
                                "MESSAGE": "**put_message_content**"
                            },
                            {
                                "MESSAGE": "**put_message_content**"
                            }
                        ]
                    }
                ],
                "ANONYMITY": 0,
                "OPTIONS": 0
            },
            "templateId": null
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.dir(result.data());
            }
        }
    );
    ```	

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'vote.Integration.Im.send',
        [
            'chatId' => **put_chat_id**,
            'IM_MESSAGE_VOTE_DATA' => [
                'QUESTIONS' => [
                    [
                        'QUESTION' => '**put_question_title**',
                        'FIELD_TYPE' => 0,
                        'ANSWERS' => [
                            ['MESSAGE' => '**put_message_content**'],
                            ['MESSAGE' => '**put_message_content**'],
                            ['MESSAGE' => '**put_message_content**']
                        ]
                    ]
                ],
                'ANONYMITY' => 0,
                'OPTIONS' => 0
            ],
            'templateId' => null
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "messageId": 23,
        "voteId": 7
    },
    "time": {
        "start": 1754470016.954889,
        "finish": 1754470017.043656,
        "duration": 0.08876705169677734,
        "processing": 0.07431983947753906,
        "date_start": "2025-08-06T11:46:56+02:00",
        "date_finish": "2025-08-06T11:46:57+02:00",
        "operating_reset_at": 1754470616,
        "operating": 0.3257129192352295
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md)| The root element of the response. Contains information about the message and vote numbers, the structure is described [below](#result) ||
|| **time**
[`time`](../data-types.md#time)| Information about the execution time of the request ||
|#

#### Response Element result {#result}
#|
|| **Name**
`type` | **Description** ||
|| **messageId**
[`integer`](../data-types.md)| The identifier of the sent message in the chat ||
|| **voteId**
[`integer`](../data-types.md)| The identifier of the created vote ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "400",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `403` | `Creating a survey is not available` ||
|| `400` | `Please enter a question and answer options` ||
|| `400` | `Maximum number of questions: 1` ||
|| `400` | `Minimum number of answers: 2` ||
|| `400` | `Maximum number of answers: 10` ||
|| `400` | `Failed to save survey data. Please try again` ||
|| `403` | `Insufficient rights to create a survey` ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./vote.attachedvote.download.md)
- [{#T}](./vote.attachedvote.get.md)
- [{#T}](./vote.attachedvote.getAnswerVoted.md)
- [{#T}](./vote.attachedvote.getMany.md)
- [{#T}](./vote.attachedvote.getWithVoted.md)
- [{#T}](./vote.attachedvote.recall.md)
- [{#T}](./vote.attachedvote.resume.md)
- [{#T}](./vote.attachedvote.stop.md)
- [{#T}](./vote.attachedvote.vote.md)