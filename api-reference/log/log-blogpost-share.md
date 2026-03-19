# Add Recipients to News Feed Message log.blogpost.share

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: administrator or message author

The method `log.blogpost.share` adds new recipients to a news feed message.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_ID***
[`integer`](../data-types.md) | Identifier of the message.

You can obtain the identifier using the [log.blogpost.get](./log-blogpost-get.md) method ||
|| **DEST***
[`array`](../data-types.md) | List of new recipients who will be granted access to view the message. 

Possible values:

{% include notitle [message recipients](./_includes/log-recepients.md) %}
||
|| **USER_ID**
[`integer`](../data-types.md) | Identifier of the user on behalf of whom the message is edited. Available only to administrators.

The identifier can be obtained using the [user.get](../user/user-get.md) method.

By default, it is the current user who initiated the method call.
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":217,"DEST":["SG69","DR4"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.share
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":217,"DEST":["SG69","DR4"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogpost.share
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'log.blogpost.share',
            {
                POST_ID: 217,
                DEST: ['SG69', 'DR4'],
            }
        );
        
        const result = response.getData().result;
        console.log(result);
        
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
                'log.blogpost.share',
                [
                    'POST_ID' => 217,
                    'DEST' => ['SG69', 'DR4']
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sharing blog post: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'log.blogpost.share',
        {
            POST_ID: 217,
            DEST: ['SG69', 'DR4']
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
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
        'log.blogpost.share',
        [
            'POST_ID' => 217,
            'DEST' => ['SG69', 'DR4']
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
    "result": true,
    "time": {
        "start": 1773814878,
        "finish": 1773814878.602921,
        "duration": 0.6029210090637207,
        "processing": 0,
        "date_start": "2026-03-18T09:21:18+02:00",
        "date_finish": "2026-03-18T09:21:18+02:00",
        "operating_reset_at": 1773815478,
        "operating": 0.284437894821167
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if recipients were added ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Wrong destinations"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong post ID` | Invalid `POST_ID` ||
|| — | `Blog module not installed` | `blog` module is not installed ||
|| — | `Wrong destinations` | Invalid list of recipients `DEST` ||
|| — | `No read perms` | Insufficient permissions to read the message ||
|| — | No access to one or more message recipients | No permissions for one or more recipients from `DEST` ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./log-blogpost-add.md)
- [{#T}](./log-blogpost-update.md)
- [{#T}](./log-blogpost-get.md)
- [{#T}](./log-blogpost-delete.md)
- [{#T}](./log-blogpost-getusers-important.md)