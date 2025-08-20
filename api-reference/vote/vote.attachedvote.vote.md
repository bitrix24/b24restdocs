# Vote in the attached voting vote.AttachedVote.vote

> Scope: [`vote`](../scopes/permissions.md)
>
> Who can execute the method: user with voting participation rights

The method `vote.AttachedVote.vote` allows you to vote in an attached voting.

## Method Parameters

There are two options for calling the method.

### 1. By the attached poll ID

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **attachId***  
[`integer`](../data-types.md) | The ID of the attached voting, which can be obtained using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md) ||
|| **ballot***  
[`object`](../data-types.md) | Voting data in the format:

```json
{
    "1": ["2", "3"],
    "2": ["5"]
}
```

Key - question ID, value - array of selected answer IDs. You can obtain the question and answer IDs using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md).
Multiple questions in one voting are available in feed posts; only one question is available in the chat message poll ||
|#

### 2. By the poll element

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***  
[`string`](../data-types.md) | Module ID, possible values:
- `Im` for chat poll,
- `blog` for feed poll ||
|| **entityType***  
[`string`](../data-types.md) | Object type, possible values:
- `Bitrix\\Vote\\Attachment\\ImMessageConnector` for chat poll,
- `Bitrix\\Vote\\Attachment\\BlogPostConnector` for feed poll ||
|| **entityId***  
[`integer`](../data-types.md) | Element ID, possible values:
- `id` of the chat message with the poll, which can be obtained using the method [vote.Integration.Im.send](./vote.integration.im.send.md),
- `id` of the post with the poll in the feed, which can be obtained using the method [log.blogpost.get](../log/log-blogpost-get.md) ||
|| **ballot***  
[`object`](../data-types.md) | Voting data in the format:

```json
{
    "1": ["2", "3"],
    "2": ["5"]
}
```

Key - question ID, value - array of selected answer IDs. You can obtain the question and answer IDs using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md).
Multiple questions in one voting are available in feed posts; only one question is available in the chat message poll ||
|#

### 3. By the signed identifier

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **signedAttachId***  
[`string`](../data-types.md) | Signed identifier of the attachment, which can be obtained using the method [vote.AttachedVote.get](./vote.attachedvote.get.md), response parameter `signedAttachId` ||
|| **ballot***  
[`object`](../data-types.md) | Voting data in the format:

```json
{
    "1": ["2", "3"],
    "2": ["5"]
}
```

Key - question ID, value - array of selected answer IDs. You can obtain the question and answer IDs using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md).
Multiple questions in one voting are available in feed posts; only one question is available in the chat message poll ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"attachId":**put_attach_id**,"ballot":{"1":["2","3"],"2":["5"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/vote.AttachedVote.vote
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"attachId":**put_attach_id**,"ballot":{"1":["2","3"],"2":["5"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/vote.AttachedVote.vote
    ```

- JS

    ```js  
    try
    {
        const response = await $b24.callMethod(
            'vote.AttachedVote.vote',
            {
                attachId: **put_attach_id**,
                ballot: {
                    '1': ['2', '3'],
                    '2': ['5']
                }
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
                'vote.AttachedVote.vote',
                [
                    'attachId' => **put_attach_id**,
                    'ballot' => [
                        '1' => ['2', '3'],
                        '2' => ['5']
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding product row: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "vote.AttachedVote.vote",
        {
            "attachId": **put_attach_id**,
            "ballot": {
                "1": ["2", "3"],
                "2": ["5"]
            }
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
        'vote.AttachedVote.vote',
        [
            'attachId' => **put_attach_id**,
            'ballot' => [
                '1' => ['2', '3'],
                '2' => ['5']
            ]
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
        "attach": {
            "ID": 1,
            "VOTE_ID": 1,
            "COUNTER": 1,
            "QUESTIONS": {
                "1": {
                    "ID": "1",
                    "ACTIVE": "Y",
                    "TIMESTAMP_X": "2025-08-05T09:08:31+02:00",
                    "VOTE_ID": "1",
                    "C_SORT": "10",
                    "COUNTER": 1,
                    "QUESTION": "How are you?",
                    "QUESTION_TYPE": "text",
                    "IMAGE_ID": null,
                    "DIAGRAM": "Y",
                    "DIAGRAM_TYPE": "histogram",
                    "REQUIRED": "N",
                    "FIELD_TYPE": "0",
                    "FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
                    "IMAGE": null,
                    "ANSWERS": {
                        "1": {
                            "ID": "1",
                            "ACTIVE": "Y",
                            "TIMESTAMP_X": "2025-08-05T09:08:31+02:00",
                            "QUESTION_ID": "1",
                            "C_SORT": "10",
                            "IMAGE_ID": null,
                            "MESSAGE": "Great!",
                            "MESSAGE_TYPE": "text",
                            "COUNTER": 1,
                            "FIELD_TYPE": "0",
                            "FIELD_WIDTH": "0",
                            "FIELD_HEIGHT": "0",
                            "FIELD_PARAM": null,
                            "COLOR": "",
                            "REACTION": "",
                            "~FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
                            "FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
                            "MESSAGE_FIELD_NAME": "bx_vote_event[1][MESSAGE][1][1]",
                            "~PERCENT": 100,
                            "PERCENT": 100
                        },
                        "3": {
                            "ID": "3",
                            "ACTIVE": "Y",
                            "TIMESTAMP_X": "2025-08-05T09:08:31+02:00",
                            "QUESTION_ID": "1",
                            "C_SORT": "20",
                            "IMAGE_ID": null,
                            "MESSAGE": "The best!",
                            "MESSAGE_TYPE": "text",
                            "COUNTER": "0",
                            "FIELD_TYPE": "0",
                            "FIELD_WIDTH": "0",
                            "FIELD_HEIGHT": "0",
                            "FIELD_PARAM": null,
                            "COLOR": "",
                            "REACTION": "",
                            "~FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
                            "FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
                            "MESSAGE_FIELD_NAME": "bx_vote_event[1][MESSAGE][1][3]",
                            "~PERCENT": 0,
                            "PERCENT": 0
                        },
                        "5": {
                            "ID": "5",
                            "ACTIVE": "Y",
                            "TIMESTAMP_X": "2025-08-05T09:08:31+02:00",
                            "QUESTION_ID": "1",
                            "C_SORT": "30",
                            "IMAGE_ID": null,
                            "MESSAGE": "What's up??",
                            "MESSAGE_TYPE": "text",
                            "COUNTER": "0",
                            "FIELD_TYPE": "0",
                            "FIELD_WIDTH": "0",
                            "FIELD_HEIGHT": "0",
                            "FIELD_PARAM": null,
                            "COLOR": "",
                            "REACTION": "",
                            "~FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
                            "FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
                            "MESSAGE_FIELD_NAME": "bx_vote_event[1][MESSAGE][1][5]",
                            "~PERCENT": 0,
                            "PERCENT": 0
                        }
                    }
                }
            },
            "ANONYMITY": 1,
            "OPTIONS": 1,
            "userAnswerMap": {
                "1": {
                    "1": {
                        "EVENT_ID": "27",
                        "EVENT_QUESTION_ID": "27",
                        "ANSWER_ID": "1",
                        "ID": "27",
                        "MESSAGE": ""
                    }
                }
            },
            "canEdit": true,
            "canVote": false,
            "canRevote": true,
            "isVoted": true,
            "signedAttachId": "1.52280488561bf567f33cf03b5ce353e154a9c3552b51b2ffb67be3bb7f26f986",
            "resultUrl": "/vote-result/me7hn922bm011mbf",
            "downloadUrl": "/bitrix/services/main/ajax.php?action=vote.AttachedVote.download&SITE_ID=s1&signedAttachId=1.52280488561bf567f33cf03b5ce353e154a9c3552b51b2ffb67be3bb7f26f986",
            "entityId": 13,
            "isFinished": false
        }
    },
    "time": {
        "start": 1754466663.090774,
        "finish": 1754466663.180457,
        "duration": 0.08968305587768555,
        "processing": 0.07362222671508789,
        "date_start": "2025-08-06T10:51:03+02:00",
        "date_finish": "2025-08-06T10:51:03+02:00",
        "operating_reset_at": 1754467263,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response. Contains the `attach` object with information about the voting, structure described [below](#attach) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Response Element attach {#attach}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | ID of the attached voting ||
|| **VOTE_ID**
[`integer`](../data-types.md) | ID of the voting itself ||
|| **COUNTER**
[`integer`](../data-types.md) | Vote counter ||
|| **QUESTIONS**
[`array`](../data-types.md) | Array of poll questions ||
|| **ANONYMITY**
[`integer`](../data-types.md) | Level of poll anonymity ||
|| **OPTIONS**
[`integer`](../data-types.md) | Availability of revoting ||
|| **userAnswerMap**
[`array`](../data-types.md) | Map of current user's answers ||
|| **canEdit**
[`bool`](../data-types.md) | Can the current user edit the poll ||
|| **canVote**
[`bool`](../data-types.md) | Can the current user vote ||
|| **canRevote**
[`bool`](../data-types.md) | Can the current user revote ||
|| **isVoted**
[`bool`](../data-types.md) | Has the current user already voted ||
|| **signedAttachId**
[`string`](../data-types.md) | Signed identifier ||
|| **resultUrl**
[`string`](../data-types.md) | URL to view poll results ||
|| **downloadUrl**
[`string`](../data-types.md) | URL to download poll results ||
|| **entityId**
[`integer`](../data-types.md) | ID of the element to which the poll is attached ||
|| **isFinished**
[`bool`](../data-types.md) | Is the poll finished ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": "403",
    "error_description": "The poll is inactive."
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `400` | Failed to save the voting ||
|| `ATTACH_READ_ACCESS_DENIED` | No rights to participate in the voting ||
|| `403` | The poll is inactive ||
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
- [{#T}](./vote.integration.im.send.md)