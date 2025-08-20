# Get Multiple Votes vote.AttachedVote.getMany

> Scope: [`vote`](../scopes/permissions.md)
>
> Who can execute the method: user with read access to votes

The method `vote.AttachedVote.getMany` returns data for multiple votes based on the identifiers of the entities to which the vote is attached.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../data-types.md) | Module identifier, possible values:
- `im` for chat poll,
- `blog` for feed poll ||
|| **entityType***
[`string`](../data-types.md) | Object type, possible values:
- `Bitrix\\Vote\\Attachment\\ImMessageConnector` for chat poll,
- `Bitrix\\Vote\\Attachment\\BlogPostConnector` for feed poll ||
|| **entityId***
[`integer`](../data-types.md) | Array of entity identifiers, possible values:
- `id` of the chat message with the poll, can be obtained using the method [vote.Integration.Im.send](./vote.integration.im.send.md),
- `id` of the post with the poll in the feed, can be obtained using the method [log.blogpost.get](../log/log-blogpost-get.md)

Maximum 50 entities ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"im","entityType":"Bitrix\\Vote\\Attachment\\ImMessageConnector","entityIds":[1,2,3]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/vote.AttachedVote.getMany
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"im","entityType":"Bitrix\\Vote\\Attachment\\ImMessageConnector","entityIds":[1,2,3],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/vote.AttachedVote.getMany
    ```

- JS

    ```js  
    try
    {
        const response = await $b24.callMethod(
            'vote.AttachedVote.getMany',
            {
                moduleId: 'im',
                entityType: 'Bitrix\\Vote\\Attachment\\ImMessageConnector',
                entityIds: [1, 2, 3]
            }
        );
        
        const result = response.getData().result;
        console.log('Data:', result);
        
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
                'vote.AttachedVote.getMany',
                [
                    'moduleId' => 'im',
                    'entityType' => 'Bitrix\\Vote\\Attachment\\ImMessageConnector',
                    'entityIds' => [1, 2, 3]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "vote.AttachedVote.getMany",
        {
            "moduleId": "im",
            "entityType": "Bitrix\\Vote\\Attachment\\ImMessageConnector",
            "entityIds": [1, 2, 3]
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
        'vote.AttachedVote.getMany',
        [
            'moduleId' => 'im',
            'entityType' => 'Bitrix\\Vote\\Attachment\\ImMessageConnector',
            'entityIds' => [1, 2, 3]
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
      "items": [
        {
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
              "COUNTER": "1",
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
                  "COUNTER": "1",
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
                  "MESSAGE": "Best of all!",
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
                "EVENT_ID": "1",
                "EVENT_QUESTION_ID": "1",
                "ANSWER_ID": "1",
                "ID": "1",
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
      ]
    },
    "time": {
      "start": 1754402559.791454,
      "finish": 1754402559.82089,
      "duration": 0.02943587303161621,
      "processing": 0.017578840255737305,
      "date_start": "2025-08-05T17:02:39+02:00",
      "date_finish": "2025-08-05T17:02:39+02:00",
      "operating_reset_at": 1754403159,
      "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response. Contains information about the requested votes, structure described [below](#result) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Response Element result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the attached vote ||
|| **VOTE_ID**
[`integer`](../data-types.md) | Identifier of the vote itself ||
|| **COUNTER**
[`integer`](../data-types.md) | Vote counter ||
|| **QUESTIONS**
[`array`](../data-types.md) | Array of poll questions ||
|| **ANONYMITY**
[`integer`](../data-types.md) | Level of poll anonymity ||
|| **OPTIONS**
[`integer`](../data-types.md) | Availability of revoting ||
|| **userAnswerMap**
[`array`](../data-types.md) | Map of the current user's answers ||
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
[`string`](../data-types.md) | URL to view the poll results ||
|| **downloadUrl**
[`string`](../data-types.md) | URL to download the poll results ||
|| **entityId**
[`integer`](../data-types.md) | Identifier of the entity to which the poll is attached ||
|| **isFinished**
[`bool`](../data-types.md) | Is the poll completed ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Too many entity ids"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Types

#|
|| **Error** | **Description** ||
|| `Too many entity ids` | Exceeded the limit of identifiers, maximum 50 ||
|| `Attach with entityId X not found` | Vote with the specified identifier not found ||
|| `Attach with entityId X read access denied` | No permission to read the vote with the specified identifier ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./vote.attachedvote.download.md)
- [{#T}](./vote.attachedvote.get.md)
- [{#T}](./vote.attachedvote.getAnswerVoted.md)
- [{#T}](./vote.attachedvote.getWithVoted.md)
- [{#T}](./vote.attachedvote.recall.md)
- [{#T}](./vote.attachedvote.resume.md)
- [{#T}](./vote.attachedvote.stop.md)
- [{#T}](./vote.attachedvote.vote.md)
- [{#T}](./vote.integration.im.send.md)