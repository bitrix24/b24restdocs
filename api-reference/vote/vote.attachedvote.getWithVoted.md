# Get Voting Data with Voter Information vote.AttachedVote.getWithVoted

> Scope: [`vote`](../scopes/permissions.md)
>
> Who can execute the method: any user with read access permission for voting

The method `vote.AttachedVote.getWithVoted` returns data for the attached vote along with information about the users who voted.

## Method Parameters

There are three options for calling the method.

### 1. By Attached Poll ID

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **attachId***
[`integer`](../data-types.md) | The ID of the attached vote, which can be obtained using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md) ||
|| **pageSize**
[`integer`](../data-types.md) | The number of records per page for the list of voters. 
Default value: `10` ||
|| **userForMobileFormat**
[`boolean`](../data-types.md) | User data format for mobile devices. 
Default value: `false` ||
|#

### 2. By Poll Element

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../data-types.md) | The module ID, possible values:
- `Im` for chat poll,
- `blog` for feed poll ||
|| **entityType***
[`string`](../data-types.md) | The object type, possible values:
- `Bitrix\\Vote\\Attachment\\ImMessageConnector` for chat poll,
- `Bitrix\\Vote\\Attachment\\BlogPostConnector` for feed poll ||
|| **entityId***
[`integer`](../data-types.md) | The element ID, possible values:
- `id` of the chat message with the poll, which can be obtained using the method [vote.Integration.Im.send](./vote.integration.im.send.md),
- `id` of the post with the poll in the feed, which can be obtained using the method [log.blogpost.get](../log/log-blogpost-get.md) ||
|| **pageSize**
[`integer`](../data-types.md) | The number of records per page for the list of voters. 
Default value: `10` ||
|| **userForMobileFormat**
[`boolean`](../data-types.md) | User data format for mobile devices. 
Default value: `false` ||
|#

### 3. By Signed ID

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **signedAttachId***
[`string`](../data-types.md) | The signed ID of the attachment, which can be obtained using the method [vote.AttachedVote.get](./vote.attachedvote.get.md), response parameter `signedAttachId` ||
|| **pageSize**
[`integer`](../data-types.md) | The number of records per page for the list of voters. 
Default value: `10` ||
|| **userForMobileFormat**
[`boolean`](../data-types.md) | User data format for mobile devices. 
Default value: `false` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"attachId":**put_attach_id**,"pageSize":**put_page_size**,"userForMobileFormat":false}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/vote.AttachedVote.getWithVoted
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"attachId":**put_attach_id**,"pageSize":**put_page_size**,"userForMobileFormat":false,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/vote.AttachedVote.getWithVoted
    ```

- JS

    ```js  
    try
    {
        const response = await $b24.callMethod(
            'vote.AttachedVote.getWithVoted',
            {
                attachId: **put_attach_id**,
                pageSize: **put_page_size**,
                userForMobileFormat: false
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
                'vote.AttachedVote.getWithVoted',
                [
                    'attachId' => **put_attach_id**,
                    'pageSize' => **put_page_size**,
                    'userForMobileFormat' => false
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
        "vote.AttachedVote.getWithVoted",
        {
            "attachId": **put_attach_id**,
            "pageSize": **put_page_size**,
            "userForMobileFormat": false
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
        'vote.AttachedVote.getWithVoted',
        [
            'attachId' => **put_attach_id**,
            'pageSize' => **put_page_size**,
            'userForMobileFormat' => false
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
        "attach": {
            "ID": 1,
            "VOTE_ID": 1,
            "COUNTER": 1,
            "QUESTIONS": {
                "1": {
                    "ID": "1",
                    "ACTIVE": "Y",
                    "TIMESTAMP_X": "2025-08-12T11:47:50+02:00",
                    "VOTE_ID": "1",
                    "C_SORT": "10",
                    "COUNTER": "1",
                    "QUESTION": "First test vote",
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
                            "TIMESTAMP_X": "2025-08-12T11:47:50+02:00",
                            "QUESTION_ID": "1",
                            "C_SORT": "10",
                            "IMAGE_ID": null,
                            "MESSAGE": "first option",
                            "MESSAGE_TYPE": "html",
                            "COUNTER": "0",
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
                            "TIMESTAMP_X": "2025-08-12T11:47:50+02:00",
                            "QUESTION_ID": "1",
                            "C_SORT": "20",
                            "IMAGE_ID": null,
                            "MESSAGE": "second option",
                            "MESSAGE_TYPE": "html",
                            "COUNTER": "1",
                            "FIELD_TYPE": "0",
                            "FIELD_WIDTH": "0",
                            "FIELD_HEIGHT": "0",
                            "FIELD_PARAM": null,
                            "COLOR": "",
                            "REACTION": "",
                            "~FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
                            "FIELD_NAME": "bx_vote_event[1][BALLOT][1]",
                            "MESSAGE_FIELD_NAME": "bx_vote_event[1][MESSAGE][1][3]",
                            "~PERCENT": 100,
                            "PERCENT": 100
                        },
                        "5": {
                            "ID": "5",
                            "ACTIVE": "Y",
                            "TIMESTAMP_X": "2025-08-12T11:47:50+02:00",
                            "QUESTION_ID": "1",
                            "C_SORT": "30",
                            "IMAGE_ID": null,
                            "MESSAGE": "third option",
                            "MESSAGE_TYPE": "html",
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
            "ANONYMITY": 0,
            "OPTIONS": 0,
            "userAnswerMap": {
                "1": {
                    "3": {
                        "EVENT_ID": "1",
                        "EVENT_QUESTION_ID": "1",
                        "ANSWER_ID": "3",
                        "ID": "1",
                        "MESSAGE": ""
                    }
                }
            },
            "canEdit": true,
            "canVote": false,
            "canRevote": false,
            "isVoted": true,
            "signedAttachId": "1.d6db75c1a03fe2313547841960f1d4e95906721cf211763be19855fc6aeec45b",
            "resultUrl": "\/vote-result\/qgp3r31sg4vj5vas",
            "downloadUrl": "\/bitrix\/services\/main\/ajax.php?action=vote.AttachedVote.download\u0026SITE_ID=s1\u0026signedAttachId=1.d6db75c1a03fe2313547841960f106721cf211763be19855fc6aeec45b",
            "entityId": 32219,
            "isFinished": false
        },
        "voted": {
            "3": [
                {
                    "ID": 1,
                    "NAME": "Alex",
                    "IMAGE": "https:\/\/your-domain.bitrix24.com\/b13743910\/resize_cache\/2267\/cec8d72046af30148f6f5b573a3a0aa8\/main\/c7b\/c7bd44b1bab25dd97d038ce1b\/d5fb56b94dc2c3cd8c006a2c595a4895.jpg",
                    "WORK_POSITION": ""
                }
            ]
        }
    },
    "time": {
        "start": 1755077476.824466,
        "finish": 1755077476.928421,
        "duration": 0.10395503044128418,
        "processing": 0.10184097290039062,
        "date_start": "2025-08-13T12:31:16+02:00",
        "date_finish": "2025-08-13T12:31:16+02:00",
        "operating_reset_at": 1755078076,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. Contains information about the voting and the voters, structure described [below](#result) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Response Element result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **attach**
[`attached_vote`](../data-types.md) | Data of the attached vote, structure described [below](#attach) ||
|| **voted**
[`object`](../data-types.md) | Information about the users who voted, structure described [below](#voted) ||
|#

##### Response Element attach {#attach}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | The ID of the attached vote ||
|| **VOTE_ID**
[`integer`](../data-types.md) | The ID of the vote itself ||
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
[`string`](../data-types.md) | Signed ID ||
|| **resultUrl**
[`string`](../data-types.md) | URL to view the poll results ||
|| **downloadUrl**
[`string`](../data-types.md) | URL to download the poll results ||
|| **entityId**
[`integer`](../data-types.md) | The ID of the element to which the poll is attached ||
|| **isFinished**
[`bool`](../data-types.md) | Is the poll completed ||
|#

##### Response Element voted {#voted}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | User ID ||
|| **NAME**
[`string`](../data-types.md) | User name ||
|| **IMAGE**
[`string`](../data-types.md) | Link to user image ||
|| **WORK_POSITION**
[`string`](../data-types.md) | User position ||
|#

## Error Handling

HTTP Status: **4xx**

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
|| `ATTACH_READ_ACCESS_DENIED` | No permission to participate in the voting ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./vote.attachedvote.download.md)
- [{#T}](./vote.attachedvote.get.md)
- [{#T}](./vote.attachedvote.getAnswerVoted.md)
- [{#T}](./vote.attachedvote.getMany.md)
- [{#T}](./vote.attachedvote.recall.md)
- [{#T}](./vote.attachedvote.resume.md)
- [{#T}](./vote.attachedvote.stop.md)
- [{#T}](./vote.attachedvote.vote.md)
- [{#T}](./vote.integration.im.send.md)