# Delete Kanban Stage / My Plan task.stages.delete

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (should include three examples - curl, js, php)
- no error response is provided
- no success response is provided
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.stages.delete` removes a Kanban stage / My Plan. It takes the `id` of the stage as input.

The stage is checked for sufficient access permission, as well as for the absence of tasks within it.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **id^*^**
[`integer`](../../data-types.md) | Identifier of the stage to be deleted. ||
|| **isAdmin**
[`boolean`](../../data-types.md) | If set to `true`, permission checks will not occur, provided that the requester is an administrator of the account. ||
|#

Returns `true` if the stage is successfully deleted.

## Examples

{% list tabs %}

- JS
    ```js
    const stageId = 5;
    BX24.callMethod(
        'task.stages.delete',
        {
            id: stageId,
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- cURL (oAuth)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "id": 5
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.delete
    ```

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 5
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.delete
    ```

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $stageId = 5;

    // executing the request to the REST API
    $result = CRest::call(
        'task.stages.delete',
        [
            'id' => $stageId
        ]
    );

    // Handling the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
"result": true
}
```

## Error Handling

```json
{
"error": "CANT_DELETE_FIRST",
"error_description": "Cannot delete the first stage. Move the stage to delete it."
}
```

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | You cannot manage stages ||
|| `CANT_DELETE_FIRST` | Cannot delete the first stage. Move the stage to delete it. ||
|| `IS_SYSTEM` | The default stage cannot be deleted ||
|| `NOT_FOUND` | Stage not found ||
|#