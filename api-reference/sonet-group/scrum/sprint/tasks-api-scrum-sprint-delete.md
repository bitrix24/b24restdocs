# Delete Sprint tasks.api.scrum.sprint.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

The method `tasks.api.scrum.sprint.delete` deletes a sprint.

When a sprint with tasks is deleted, the tasks will be moved to the backlog.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **id^*^**
[`integer`](../../../data-types.md) | Identifier of the sprint. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    const sprintId = 1;
    BX24.callMethod(
        'tasks.api.scrum.sprint.delete',
        {
            id: sprintId
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
    "id": 1
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.delete
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 1
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.delete
    ```

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.delete',
        [
            'id' => 1,
        ]
    );

    // Handling the response from Bitrix24
    if (isset($result['error'])) {
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
  "result" : []
}
```

Upon successful deletion, the method returns an empty array.

## Error Handling

HTTP Status: **200**

```json
{
"error": 0,
"error_description": "Sprint not found"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access denied | No access to Scrum ||
|| `0` | Sprint not found | This sprint does not exist ||
|| `0` | It is forbidden to remove a sprint with items | Cannot delete a sprint that has tasks ||
|| `0` | Sprint items have not been moved to backlog | Failed to move tasks from the sprint to the backlog ||
|| `100` | Could not find value for parameter {id} | Incorrect parameter name or parameter not set ||
|| `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | Invalid parameter type ||
|#

{% include [Example Note](../../../../_includes/examples.md) %}