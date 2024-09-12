# Change the Stage of Kanban / My Plan task.stages.update

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.stages.update` updates the stages of Kanban / My Plan. It takes the `id` of the stage and an array of `fields` as input.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **id^*^**
[`integer`](../../data-types.md) | Identifier of the stage. ||
|| **fields^*^**
[`array`](../../data-types.md) | Array for updating, similar to the array used in [task.stages.add](./task-stages-add.md), except for the `ENTITY_ID` field — it cannot be changed. Access permission checks are performed similarly to `task.stages.add`. ||
|| **isAdmin**
[`boolean`](../../data-types.md) | If set to `true`, permission checks will not be performed, provided that the requester is an administrator of the account. ||
|#

The method can also be used to move a stage from one position to another — simply pass the desired `AFTER_ID`.

Returns `true` in case of success.

## Examples

{% list tabs %}

- JS
    ```js
    const stageId = 5;
    const fields = {
        TITLE: "New Stage",
        SORT: 200,
        COLOR: "FF5733"
    };
    BX24.callMethod(
        'task.stages.update',
        {
            id: stageId,
            fields: fields
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
    "id": 5,
    "fields": {
        "TITLE": "New Stage",
        "SORT": 200,
        "COLOR": "FF5733"
    }
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.update
    ```

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 5,
    "fields": {
        "TITLE": "New Stage",
        "SORT": 200,
        "COLOR": "FF5733"
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.update
    ```

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $stageId = 5;
    $fields = [
        "TITLE" => "New Stage",
        "SORT" => 200,
        "COLOR" => "FF5733"
    ];

    // executing the request to the REST API
    $result = CRest::call(
        'task.stages.update',
        [
            'id' => $stageId,
            'fields' => $fields
        ]
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: 200

```json
{
"result": true
}
```

## Error Handling

HTTP status: **200**

```json
{
"error": "ACCESS_DENIED",
"error_description": "You cannot modify stages in this group"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | You cannot modify stages in this group ||
|| `NOT_FOUND` | Stage not found ||
|#