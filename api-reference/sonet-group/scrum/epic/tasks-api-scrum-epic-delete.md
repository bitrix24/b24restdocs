# Delete Epic tasks.api.scrum.epic.delete

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

The method `tasks.api.scrum.epic.delete` deletes an epic.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **id^*^**
[`integer`](../../../data-types.md) | Identifier of the epic. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    const epicId = 1;
    BX24.callMethod(
        'tasks.api.scrum.epic.delete',
        {
            id: epicId,
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
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.epic.delete
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 1
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.delete
    ```

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing request to REST API
    $result = CRest::call(
        'tasks.api.scrum.epic.delete',
        [
            'id' => 1,
        ]
    );

    // Handling response from Bitrix24
    if (isset($result['error'])) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

Upon successful deletion, the method returns an empty array.

## Error Handling

HTTP status: **200**

```json
{
  "error": 0,
  "error_description": "Epic not found"
}
```

### Possible Error Codes

#|
|| **Code** | **Description**  | **Value** ||
|| `0` | Access denied | No access to Scrum ||
|| `0` | Epic not found | The epic does not exist ||
|| `100` | Could not find value for parameter {id} | Incorrect parameter name or parameter not set ||
|| `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | Invalid parameter type ||
|#

{% include [Example Note](../../../../_includes/examples.md) %}