# Update Sprint tasks.api.scrum.sprint.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

The method `tasks.api.scrum.sprint.update` updates a sprint.

All sprint fields are available for updating. Non-updatable fields can be omitted.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **id^*^**
[`integer`](../../../data-types.md) | Sprint identifier. ||
|| **fields^*^**
[`object`](../../../data-types.md) | Object containing sprint data. ||
|#

### Parameter fields

#|
|| **Parameter** / **Type** | **Description** ||
|| **groupId** `integer` | Identifier of the group (scrum) to which the sprint belongs. Can be obtained by calling the method [tasks.api.scrum.sprint.get](./tasks-api-scrum-sprint-get.md) for an existing sprint. ||
|| **name** `string` | Name of the sprint. ||
|| **sort** `integer` | Sorting order. ||
|| **dateStart** `string` | Start date of the sprint. Available formats: 'ISO 8601', timestamp. ||
|| **dateEnd** `string` | End date of the sprint. Available formats: 'ISO 8601', timestamp. ||
|| **status** `string` | Status of the sprint. Available values: 'active', 'planned', 'completed'. ||
|#

{% include [Parameter notes](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    const sprintId = 2;
    const groupId = 1;
    const name = 'Sprint 2';
    const dateStart = '2021-11-22T00:00:00+02:00';
    const dateEnd = '2021-11-29T00:00:00+02:00';
    BX24.callMethod(
        'tasks.api.scrum.sprint.update',
        {
            id: sprintId,
            fields: {
                name: name,
                groupId: groupId,
                dateStart: dateStart,
                dateEnd: dateEnd,
            }
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
    "id": 2,
    "fields": {
        "name": "Sprint 2",
        "groupId": 1,
        "dateStart": "2021-11-22T00:00:00+02:00",
        "dateEnd": "2021-11-29T00:00:00+02:00"
    }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.update
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 2,
    "fields": {
        "name": "Sprint 2",
        "groupId": 1,
        "dateStart": "2021-11-22T00:00:00+02:00",
        "dateEnd": "2021-11-29T00:00:00+02:00"
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.update
    ```

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.update',
        [
            'id' => 2,
            'fields' => [
                'name' => 'Sprint 2',
                'groupId' => 1,
                'dateStart' => '2021-11-22T00:00:00+02:00',
                'dateEnd' => '2021-11-29T00:00:00+02:00'
            ]
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
"result":
{
    "id": 2,
    "groupId": 1,
    "entityType": "sprint",
    "name": "Sprint 2",
    "goal": "",
    "sort": 1,
    "createdBy": 1,
    "modifiedBy": 1,
    "dateStart": "2021-11-22T00:00:00+02:00",
    "dateEnd": "2021-11-29T00:00:00+02:00",
    "status": "planned"
}
}
```

## Returned Data

#|
|| **Field** `type` | **Description** ||
|| **result** `object` | Object containing sprint data. ||
|| **id** `integer` | Sprint identifier. ||
|| **groupId** `integer` | Identifier of the group (scrum) to which the sprint belongs. ||
|| **entityType** `string` | Type of entity (in this case, "sprint"). ||
|| **name** `string` | Name of the sprint. ||
|| **goal** `string` | Goal of the sprint (set only in the interface when starting the sprint). ||
|| **sort** `integer` | Sorting order. ||
|| **createdBy** `integer` | Identifier of the user who created the sprint. ||
|| **modifiedBy** `integer` | Identifier of the user who modified the sprint. ||
|| **dateStart** `string` | Start date of the sprint in ISO 8601 format. ||
|| **dateEnd** `string` | End date of the sprint in ISO 8601 format. ||
|| **status** `string` | Status of the sprint. ||
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
|| `0` | Access denied | No access to scrum. ||
|| `0` | Sprint not created | Failed to create sprint. ||
|| `0` | Incorrect dateStart format | Invalid start date format for the sprint. ||
|| `0` | Incorrect dateEnd format | Invalid end date format for the sprint. ||
|| `0` | createdBy user not found | User in the "creator" field not found. ||
|| `0` | modifiedBy user not found | User in the "last modified by" field not found. ||
|| `0` | Unable to add two active sprints | There cannot be two sprints with "active" status in the group. ||
|| `0` | Incorrect sprint status | Status is not in the list of available sprint statuses. ||
|| `100` | Could not find value for parameter {fields} | Incorrect parameter name or parameter not set. ||
|| `100` | Invalid value {stringValue} to match with parameter {fields}. Should be value of type array. | Invalid parameter type. ||
|#
{% include [Example notes](../../../../_includes/examples.md) %}