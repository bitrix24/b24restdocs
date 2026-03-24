# Get the list of group participants sonet_group.user.get

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `sonet_group.user.get` returns a list of active participants in a workgroup or project.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../data-types.md) | Identifier of the workgroup or project.

The identifier can be obtained using the [sonet_group.get](../sonet-group-get.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":69}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.user.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":69,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.user.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.user.get',
            {
                ID: 69
            }
        );
        
        const result = response.getData().result;
        console.log('Group users:', result);
        
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
                'sonet_group.user.get',
                [
                    'ID' => 69
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting group users: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sonet_group.user.get',
        {
            ID: 69
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
        'sonet_group.user.get',
        [
            'ID' => 69
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
    "result": [
        {
        "USER_ID": "1269",
        "ROLE": "A"
        },
        {
        "USER_ID": "1271",
        "ROLE": "E"
        },
        {
        "USER_ID": "779",
        "ROLE": "K"
        }
    ],
    "time": {
        "start": 1773850553,
        "finish": 1773850553.261059,
        "duration": 0.261059045791626,
        "processing": 0,
        "date_start": "2026-03-18T19:15:53+02:00",
        "date_finish": "2026-03-18T19:15:53+02:00",
        "operating_reset_at": 1773851153,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of participants in the group or project ||
|| **USER_ID**
[`integer`](../../data-types.md) | Identifier of the participant ||
|| **ROLE**
[`string`](../../data-types.md) | Role of the participant.

Possible values:

- `A` — owner
- `E` — moderator
- `K` — participant  ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Socialnetwork group not found"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong socialnetwork group ID` | An incorrect `ID` of the group was provided ||
|| — | `Socialnetwork group not found` | Group or project not found or is private ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-user-invite.md)
- [{#T}](./sonet-group-user-request.md)
- [{#T}](./sonet-group-user-add.md)
- [{#T}](./sonet-group-user-update.md)
- [{#T}](./sonet-group-user-delete.md)