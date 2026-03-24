# Delete group or project sonet_group.delete

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: administrator or group or project owner

The method `sonet_group.delete` removes a workgroup or project.

## Method parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **GROUP_ID***
[`integer`](../data-types.md) | Identifier of the group or project to be deleted.

The identifier can be obtained using the [sonet_group.get](./sonet-group-get.md) method ||
|#

## Code examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":77}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":77,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.delete',
            {
                GROUP_ID: 77
            }
        );
        
        const result = response.getData().result;
        console.log('Deleted group:', result);
        
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
                'sonet_group.delete',
                [
                    'GROUP_ID' => 77
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sonet_group.delete',
        {
            GROUP_ID: 77
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
        'sonet_group.delete',
        [
            'GROUP_ID' => 77
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773931621,
        "finish": 1773931622.233361,
        "duration": 1.233361005783081,
        "processing": 1,
        "date_start": "2026-03-19T17:47:01+02:00",
        "date_finish": "2026-03-19T17:47:02+02:00",
        "operating_reset_at": 1773932221,
        "operating": 0.4352729320526123
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if the group or project has been deleted ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "User has no permissions to delete group"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong group ID` | An incorrect `GROUP_ID` was provided ||
|| — | `Socialnetwork group not found` | Group or project not found ||
|| — | `User has no permissions to delete group` | Insufficient rights to delete the group ||
|| — | `Cannot delete group` | Failed to delete the group ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue exploring

- [{#T}](./sonet-group-create.md)
- [{#T}](./sonet-group-update.md)
- [{#T}](./socialnetwork-api-workgroup-get.md)
- [{#T}](./socialnetwork-api-workgroup-list.md)
- [{#T}](./sonet-group-get.md)