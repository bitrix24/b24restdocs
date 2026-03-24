# Change the owner of a group or project sonet_group.setowner

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: group or project owner

The method `sonet_group.setowner` assigns a new owner to a workgroup or project.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **GROUP_ID***
[`integer`](../data-types.md) | Identifier of the group or project.

The identifier can be obtained using the [sonet_group.get](./sonet-group-get.md) method. ||
|| **USER_ID***
[`integer`](../data-types.md) | Identifier of the new owner.

The user identifier can be obtained using the [user.get](../user/user-get.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":77,"USER_ID":1269}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.setowner
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":77,"USER_ID":1269,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.setowner
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.setowner',
            {
                GROUP_ID: 77,
                USER_ID: 1269
            }
        );
        
        const result = response.getData().result;
        console.log('Set owner for group:', result);
        
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
                'sonet_group.setowner',
                [
                    'GROUP_ID' => 77,
                    'USER_ID' => 1269
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting group owner: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sonet_group.setowner',
        {
            GROUP_ID: 77,
            USER_ID: 1269
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
        'sonet_group.setowner',
        [
            'GROUP_ID' => 77,
            'USER_ID' => 1269
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
    "result": true,
    "time": {
        "start": 1773810300,
        "finish": 1773810300.3341,
        "duration": 0.3341,
        "processing": 0.1503,
        "date_start": "2026-03-18T08:45:00+02:00",
        "date_finish": "2026-03-18T08:45:00+02:00",
        "operating_reset_at": 1773810900,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if the owner has been changed. ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "User has no permissions to set owner"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Invalid workgroup/project ID` | An invalid `GROUP_ID` was provided or the group/project was not found. ||
|| — | `Invalid user ID` | An invalid `USER_ID` was provided. ||
|| — | `Insufficient permissions to complete the operation.` | Not enough permissions to change the owner. ||
|| — | `Cannot complete operation.` | Failed to change the owner. ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-update.md)
- [{#T}](./sonet-group-user-groups.md)
- [{#T}](./socialnetwork-api-workgroup-get.md)
- [{#T}](./sonet-group-get.md)
- [{#T}](./sonet-group-delete.md)