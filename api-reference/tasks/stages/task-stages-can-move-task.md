# Check the ability to move a task task.stages.canmovetask

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- no error response
- no success response
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.stages.canmovetask` determines whether the current user can move tasks in the specified entity.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **entityId^*^**
[`integer`](../../data-types.md) | Entity ID. ||
|| **entityType^*^**
[`string`](../../data-types.md) | Entity type (`U` — user or `G` — group). In the case of `U` (My plan), `true` will only be returned if the current user's ID is passed in `entityId`. ||
|#

## Examples

{% list tabs %}

- JS
    ```js
    const entityId = 1;
    const entityType = 'U';
    BX24.callMethod(
        'task.stages.canmovetask',
        {
            entityId: entityId,
            entityType: entityType
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
    "entityId": 1,
    "entityType": "U"
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.canmovetask
    ```

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "entityId": 1,
    "entityType": "U"
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.canmovetask
    ```

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $entityId = 1;
    $entityType = 'U';

    // executing the request to the REST API
    $result = CRest::call(
        'task.stages.canmovetask',
        [
            'entityId' => $entityId,
            'entityType' => $entityType
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

The method does not return errors.