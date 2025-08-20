# Resume Voting vote.AttachedVote.resume

> Scope: [`vote`](../scopes/permissions.md)
>
> Who can execute the method: user with editing rights for the vote

The method `vote.AttachedVote.resume` resumes a stopped vote, allowing users to participate in it again.

## Method Parameters

There are three options for calling the method.

### 1. By the attached poll ID

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **attachId*** 
[`integer`](../data-types.md)| The ID of the attached vote, which can be obtained using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md) ||
|#

### 2. By the entity with the poll

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
[`integer`](../data-types.md) | The ID of the entity, possible values:
- `id` of the chat message with the poll, which can be obtained using the method [vote.Integration.Im.send](./vote.integration.im.send.md),
- `id` of the post with the poll in the feed, which can be obtained using the method [log.blogpost.get](../log/log-blogpost-get.md) ||
|#

### 3. By the signed ID

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **signedAttachId*** 
[`string`](../data-types.md) | The signed ID of the attachment, which can be obtained using the method [vote.AttachedVote.get](./vote.attachedvote.get.md), response parameter `signedAttachId` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"attachId":**put_attach_id**}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/vote.AttachedVote.resume
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"attachId":**put_attach_id**,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/vote.AttachedVote.resume
    ```

- JS

    ```js  
    try
    {
        const response = await $b24.callMethod(
            'vote.AttachedVote.resume',
            {
                attachId: **put_attach_id**
            }
        );
        
        const result = response.getData().result;
        console.log('Resumed vote with ID:', result);
        
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
                'vote.AttachedVote.resume',
                [
                    'attachId' => **put_attach_id**
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error resuming vote: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "vote.AttachedVote.resume",
        {
            "attachId": **put_attach_id**
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
        'vote.AttachedVote.resume',
        [
            'attachId' => **put_attach_id**
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
    "result": [],
    "time": {
        "start": 1754464002.144207,
        "finish": 1754464002.182058,
        "duration": 0.03785109519958496,
        "processing": 0.020069122314453125,
        "date_start": "2025-08-06T10:06:42+02:00",
        "date_finish": "2025-08-06T10:06:42+02:00",
        "operating_reset_at": 1754464602,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | An empty array upon successful operation ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
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
|| `ATTACH_NOT_FOUND` | Vote not found ||
|| `ATTACH_READ_ACCESS_DENIED` | No permission to participate in the vote ||
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
- [{#T}](./vote.attachedvote.stop.md)
- [{#T}](./vote.attachedvote.vote.md)
- [{#T}](./vote.integration.im.send.md)