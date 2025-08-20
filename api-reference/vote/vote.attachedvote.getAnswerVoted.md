# Get the list of users who voted for the answer vote.AttachedVote.getAnswerVoted

> Scope: [`vote`](../scopes/permissions.md)
>
> Who can execute the method: user with read access permission for voting

The method `vote.AttachedVote.getAnswerVoted` returns a list of users who voted for the specified answer option.

## Method Parameters

There are three ways to call the method.

### 1. By the attached poll ID

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **attachId***
[`integer`](../data-types.md) | The ID of the attached vote, which can be obtained using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md) ||
|| **answerId***
[`integer`](../data-types.md) | The ID of the answer, which can be obtained using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md) ||
|| **pageNavigation**
[`object`](../data-types.md) | Pagination parameters, for example { size: 10, page: 1 } ||
|| **userForMobileFormat**
[`boolean`](../data-types.md) | User data format for mobile devices. Default value: `false` ||
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
[`integer`](../data-types.md) | The ID of the element, possible values:
- `id` of the chat message with the poll, which can be obtained using the method [vote.Integration.Im.send](./vote.integration.im.send.md),
- `id` of the post with the poll in the feed, which can be obtained using the method [log.blogpost.get](../log/log-blogpost-get.md) ||
|| **answerId***
[`integer`](../data-types.md) | The ID of the answer, which can be obtained using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md) ||
|| **pageNavigation**
[`object`](../data-types.md) | Pagination parameters, for example { size: 10, page: 1 } ||
|| **userForMobileFormat**
[`boolean`](../data-types.md) | User data format for mobile devices. Default value: `false` ||
|#

### 3. By the signed ID

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **signedAttachId***
[`string`](../data-types.md) | The signed ID of the attachment, which can be obtained using the method [vote.AttachedVote.get](./vote.attachedvote.get.md), response parameter `signedAttachId` ||
|| **answerId***
[`integer`](../data-types.md) | The ID of the answer, which can be obtained using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md) ||
|| **pageNavigation**
[`object`](../data-types.md) | Pagination parameters, for example { size: 10, page: 1 } ||
|| **userForMobileFormat**
[`boolean`](../data-types.md) | User data format for mobile devices. Default value: `false` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"attachId":**put_attach_id**,"answerId":**put_answer_id**,"pageNavigation":{"pageSize":**put_page_size**,"currentPage":**put_page**},"userForMobileFormat":false}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/vote.AttachedVote.getAnswerVoted
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"attachId":**put_attach_id**,"answerId":**put_answer_id**,"pageNavigation":{"pageSize":**put_page_size**,"currentPage":**put_page**},"userForMobileFormat":false,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/vote.AttachedVote.getAnswerVoted
    ```

- JS

    ```js  
    try
    {
        const response = await $b24.callMethod(
            'vote.AttachedVote.getAnswerVoted',
            {
                attachId: **put_attach_id**,
                answerId: **put_answer_id**,
                pageNavigation: {
                    pageSize: **put_page_size**,
                    currentPage: **put_page**
                },
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
                'vote.AttachedVote.getAnswerVoted',
                [
                    'attachId' => **put_attach_id**,
                    'answerId' => **put_answer_id**,
                    'pageNavigation' => [
                        'pageSize' => **put_page_size**,
                        'currentPage' => **put_page**
                    ],
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
        "vote.AttachedVote.getAnswerVoted",
        {
            "attachId": **put_attach_id**,
            "answerId": **put_answer_id**,
            "pageNavigation": {
                "pageSize": **put_page_size**,
                "currentPage": **put_page**
            },
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
        'vote.AttachedVote.getAnswerVoted',
        [
            'attachId' => **put_attach_id**,
            'answerId' => **put_answer_id**,
            'pageNavigation' => [
                'pageSize' => **put_page_size**,
                'currentPage' => **put_page**
            ],
            'userForMobileFormat' => false
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
                "NAME": "Alex",
                "IMAGE": "https:\/\/your-domain.bitrix24.com\/b13743910\/resize_cache\/2267\/cec8d72046af30148f6f5b573a3a0aa8\/main\/c7b\/c7bd44b1babaa54438ce1b\/d5fb56b94dc2c3c2c595a4895.jpg",
                "WORK_POSITION": ""
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1755076703.233388,
        "finish": 1755076703.264925,
        "duration": 0.03153705596923828,
        "processing": 0.029319047927856445,
        "date_start": "2025-08-13T12:18:23+02:00",
        "date_finish": "2025-08-13T12:18:23+02:00",
        "operating_reset_at": 1755077303,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. Contains an array of data about the users who voted, the structure is described [below](#result) ||
|| **total**
[`integer`](../data-types.md) | The total number of users who voted for this answer ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Response Element result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | The user ID ||
|| **NAME**
[`string`](../data-types.md) | The user's name ||
|| **IMAGE**
[`string`](../data-types.md) | The link to the user's image ||
|| **WORK_POSITION**
[`string`](../data-types.md) | The user's position ||
|#

## Error Handling

HTTP status: **4xx** (for example, 401, 403)

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
|| `ATTACH_NOT_FOUND` | Vote not found ||
|| `ATTACH_READ_ACCESS_DENIED` | No permission to participate in the vote ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./vote.attachedvote.download.md)
- [{#T}](./vote.attachedvote.get.md)
- [{#T}](./vote.attachedvote.getMany.md)
- [{#T}](./vote.attachedvote.getWithVoted.md)
- [{#T}](./vote.attachedvote.recall.md)
- [{#T}](./vote.attachedvote.resume.md)
- [{#T}](./vote.attachedvote.stop.md)
- [{#T}](./vote.attachedvote.vote.md)
- [{#T}](./vote.integration.im.send.md)