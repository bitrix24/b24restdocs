# Get Sprint Fields by Its Identifier tasks.api.scrum.sprint.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.sprint.get` returns the values of the sprint fields by its identifier.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **sprintId^*^**
[`integer`](../../../data-types.md) | The identifier of the sprint. It can be obtained using the method [tasks.api.scrum.sprint.list](./tasks-api-scrum-sprint-list.md). ||
|#

## Examples
{% list tabs %}

- JS
    ```js
    const sprintId = 2;
    BX24.callMethod(
        'tasks.api.scrum.sprint.get',
        {
            id: sprintId,
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
    -d '{
    "id": 2,
    "auth": "YOUR_ACCESS_TOKEN"
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.get
    ```

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 2
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.get
    ```

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $sprintId = 2;

    // executing the request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.get',
        [
            'id' => $sprintId
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
  "result":
  {
    "id": 2,
    "groupId": 143,
    "entityType": "sprint",
    "name": "Sprint 1",
    "goal": "",
    "sort": 1,
    "createdBy": 1,
    "modifiedBy": 1,
    "dateStart": "2024-07-19T15:03:01+00:00",
    "dateEnd": "2024-08-02T15:03:01+00:00",
    "status": "planned"
  }
}
```

## Returned Data

#|
|| **Field** `type` | **Description** ||
|| **result** `object` | An object containing data about the sprint ||
|| **id** `integer` | The identifier of the sprint ||
|| **groupId** `integer` | The identifier of the group (scrum) to which the sprint belongs ||
|| **entityType** `string` | The type of entity (in this case, "sprint") ||
|| **name** `string` | The name of the sprint ||
|| **goal** `string` | The goal of the sprint (set only in the interface when starting the sprint) ||
|| **sort** `integer` | Sorting ||
|| **createdBy** `integer` | The identifier of the user who created the sprint ||
|| **modifiedBy** `integer` | The identifier of the user who modified the sprint ||
|| **dateStart** `string` | The start date of the sprint in ISO 8601 format ||
|| **dateEnd** `string` | The end date of the sprint in ISO 8601 format ||
|| **status** `string` | The status of the sprint ||
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
|| **Code** | **Description**  | **Value** ||
|| `0` | Access denied | No access to view sprint data ||
|| `0` | Sprint not found | The sprint does not exist ||
|| `100` | Could not find value for parameter {id} | The parameter name is incorrect or the parameter is not set ||
|| `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | Invalid parameter type ||
|#

{% include [Examples Note](../../../../_includes/examples.md) %}