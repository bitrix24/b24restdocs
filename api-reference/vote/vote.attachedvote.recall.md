# Recall Your Vote vote.AttachedVote.recall

> Scope: [`vote`](../scopes/permissions.md)
>
> Who can execute the method: any user who participated in the voting

The method `vote.AttachedVote.recall` allows a user to withdraw their vote in an active voting session. This method only works with votes where the option Allow Re-voting is enabled.

## Method Parameters

There are three ways to call the method.

### 1. By the attached poll ID

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **attachId***
[`integer`](../data-types.md)| The ID of the attached vote, which can be obtained using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md) ||
|#

### 2. By the poll element

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../data-types.md) | The module ID, possible values:
- `Im` for a poll in chat,
- `blog` for a poll in the feed ||
|| **entityType***
[`string`](../data-types.md) | The object type, possible values:
- `Bitrix\\Vote\\Attachment\\ImMessageConnector` for a poll in chat,
- `Bitrix\\Vote\\Attachment\\BlogPostConnector` for a poll in the feed ||
|| **entityId***
[`integer`](../data-types.md) | The element ID, possible values:
- `id` of the chat message with the poll, which can be obtained using the method [vote.Integration.Im.send](./vote.integration.im.send.md),
- `id` of the post with the poll in the feed, which can be obtained using the method [log.blogpost.get](../log/log-blogpost-get.md) ||
|#

### 3. By the signed ID

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **signedAttachId***
[`string`](../data-types.md) | The signed attachment ID, which can be obtained using the method [vote.AttachedVote.get](./vote.attachedvote.get.md), response parameter `signedAttachId` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"attachId":"**put_attach_id**"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/vote.AttachedVote.recall
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"attachId":"**put_attach_id**","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/vote.AttachedVote.recall
    ```

- JS

    ```js  
    try
    {
        const response = await $b24.callMethod(
            'vote.AttachedVote.recall',
            {
                attachId: '**put_attach_id**',
            }
        );
        
        const result = response.getData().result;
        console.log('Recalled vote with ID:', result);
        
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
                'vote.AttachedVote.recall',
                [
                    'attachId' => '**put_attach_id**'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error recalling vote: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "vote.AttachedVote.recall",
        {
            "attachId": "**put_attach_id**"
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
        'vote.AttachedVote.recall',
        [
            'attachId' => '**put_attach_id**'
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
      "COUNTER": 0,
      "QUESTIONS": {
        "1": {
          "ID": "1",
          "ACTIVE": "Y",
          "TIMESTAMP_X": "2025-08-05T09:08:31+02:00",
          "VOTE_ID": "1",
          "C_SORT": "10",
          "COUNTER": 0,
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
              "COUNTER": 0,
              "FIELD_TYPE": "0",
              "FIELD_WIDTH": "0",
              "FIELD_HEIGHT": "0",
              "FIELD_PARAM": null,
              "COLOR": "",
              "REACTION": "",
              "~FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
              "FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
              "MESSAGE_FIELD_NAME": "bx_vote_event[1][MESSAGE][1][1]",
              "~PERCENT": 0,
              "PERCENT": 0
            },
            "3": {
              "ID": "3",
              "ACTIVE": "Y",
              "TIMESTAMP_X": "2025-08-05T09:08:31+02:00",
              "QUESTION_ID": "1",
              "C_SORT": "20",
              "IMAGE_ID": null,
              "MESSAGE": "Better than anyone!",
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
      "userAnswerMap": [],
      "canEdit": true,
      "canVote": true,
      "canRevote": true,
      "isVoted": false,
      "signedAttachId": "1.52280488561bf567f33cf03b5ce353e154a9c3552b51b2ffb67be3bb7f26f986",
      "resultUrl": "/vote-result/me7hn922bm011mbf",
      "downloadUrl": "/bitrix/services/main/ajax.php?action=vote.AttachedVote.download&SITE_ID=s1&signedAttachId=1.52280488561bf567f33cf03b5ce353e154a9c3552b51b2ffb67be3bb7f26f986",
      "entityId": 13,
      "isFinished": false
    }
  },
  "time": {
    "start": 1754461747.345456,
    "finish": 1754461747.393074,
    "duration": 0.04761815071105957,
    "processing": 0.03604316711425781,
    "date_start": "2025-08-06T09:29:07+02:00",
    "date_finish": "2025-08-06T09:29:07+02:00",
    "operating_reset_at": 1754462347,
    "operating": 0
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. Contains the `attach` object with information about the voting, structure described [below](#attach) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Response Element attach {#attach}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | The ID of the attached vote ||
|| **VOTE_ID**
[`integer`](../data-types.md) | The ID of the vote itself ||
|| **COUNTER**
[`integer`](../data-types.md) | The vote counter ||
|| **QUESTIONS**
[`array`](../data-types.md) | An array of the poll questions ||
|| **ANONYMITY**
[`integer`](../data-types.md) | The level of anonymity of the poll ||
|| **OPTIONS**
[`integer`](../data-types.md) | Availability of re-voting ||
|| **userAnswerMap**
[`array`](../data-types.md) | Map of the current user's answers ||
|| **canEdit**
[`bool`](../data-types.md) | Can the current user edit the poll ||
|| **canVote**
[`bool`](../data-types.md) | Can the current user vote ||
|| **canRevote**
[`bool`](../data-types.md) | Can the current user re-vote ||
|| **isVoted**
[`bool`](../data-types.md) | Has the current user already voted ||
|| **signedAttachId**
[`string`](../data-types.md) | The signed ID ||
|| **resultUrl**
[`string`](../data-types.md) | URL to view the poll results ||
|| **downloadUrl**
[`string`](../data-types.md) | URL to download the poll results ||
|| **entityId**
[`integer`](../data-types.md) | The ID of the element to which the poll is attached ||
|| **isFinished**
[`bool`](../data-types.md) | Is the poll finished ||
|#

## Error Handling

HTTP status: **4xx** 

```json
{
    "error": "ATTACH_NOT_FOUND",
    "error_description": "Attach not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ATTACH_NOT_FOUND` | Voting not found ||
|| `ATTACH_READ_ACCESS_DENIED` | No permission to participate in the voting  ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./vote.attachedvote.download.md)
- [{#T}](./vote.attachedvote.get.md)
- [{#T}](./vote.attachedvote.getAnswerVoted.md)
- [{#T}](./vote.attachedvote.getMany.md)
- [{#T}](./vote.attachedvote.getWithVoted.md)
- [{#T}](./vote.attachedvote.resume.md)
- [{#T}](./vote.attachedvote.stop.md)
- [{#T}](./vote.attachedvote.vote.md)
- [{#T}](./vote.integration.im.send.md)