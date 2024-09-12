# Complete Active Sprint of Selected Scrum tasks.api.scrum.sprint.complete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

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
> Who can execute the method: any user

The method `tasks.api.scrum.sprint.complete` completes the active sprint of the selected Scrum.

When the sprint is completed, unfinished tasks are moved to the backlog.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **id^*^**
[`integer`](../../../data-types.md) | Identifier of the group with the active sprint. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    const groupId = 1;
    BX24.callMethod(
        'tasks.api.scrum.sprint.complete',
        {
            id: groupId
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
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.complete
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 1
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.complete
    ```

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing request to REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.complete',
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

HTTP Status: **200**

```json
{
  "result":
  {
    "id": 1,
    "groupId": 143,
    "entityType": "sprint",
    "name": "Sprint 1",
    "goal": "Goal",
    "sort": 1,
    "createdBy": 1,
    "modifiedBy": 1,
    "dateStart": "2024-07-19T15:03:01+00:00",
    "dateEnd": "2024-08-02T15:03:01+00:00",
    "status": "completed"
  }
}
```

## Returned Data

#|
|| **Field** `type` | **Description** ||
|| **result** `object` | Object containing data about the sprint ||
|| **id** `integer` | Identifier of the sprint ||
|| **groupId** `integer` | Identifier of the group (scrum) to which the sprint belongs ||
|| **entityType** `string` | Type of entity (in this case, "sprint") ||
|| **name** `string` | Name of the sprint ||
|| **goal** `string` | Goal of the sprint (set only in the interface when starting the sprint) ||
|| **sort** `integer` | Sorting ||
|| **createdBy** `integer` | Identifier of the user who created the sprint ||
|| **modifiedBy** `integer` | Identifier of the user who modified the sprint ||
|| **dateStart** `string` | Start date of the sprint in ISO 8601 format ||
|| **dateEnd** `string` | End date of the sprint in ISO 8601 format ||
|| **status** `string` | Status of the sprint ||
|#

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
|| `0` | Access denied | No access to the scrum ||
|| `0` | Sprint not found | Active sprint not found in the group ||
|| `100` | Could not find value for parameter {id} | Incorrect parameter name or parameter not set ||
|| `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | Invalid parameter type ||
|#
{% include [Example Note](../../../../_includes/examples.md) %}