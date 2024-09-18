# Get a List of Sprints tasks.api.scrum.sprint.list

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.sprint.list` returns a list of sprints.

This method is similar to other methods with filtering by list.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the result. The object format is `{'sorting_field': 'sorting_direction' [, ...]}`. Available fields are described in the table [below](#fields).

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending ||
|| **filter**
[`object`](../../../data-types.md) | An object in the format `{'filter_field': 'filter_value' [, ...]}`. Available fields are described in the table [below](#fields) ||
|| **select**
[`object`](../../../data-types.md) | An array of record fields that will be returned by the method. You can specify only the fields that are necessary. 

If the array contains the value `"*"`, all available fields will be returned.

The default value is an empty array `array()`. In this case, all fields of the main query table will be returned. ||
|| **start**
[`integer`](../../../data-types.md) | The page number of the output. Works for https requests ||
|#

### Available Filter Fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID** 
[`integer`](../../../data-types.md) | Sprint identifier ||
|| **GROUP_ID** 
[`integer`](../../../data-types.md) | Scrum identifier ||
|| **ENTITY_TYPE** 
[`string`](../../../data-types.md) | Entity type ||
|| **NAME** 
[`string`](../../../data-types.md) | Name ||
|| **SORT** 
[`integer`](../../../data-types.md) | Sorting ||
|| **CREATED_BY** 
[`integer`](../../../data-types.md) | Created by ||
|| **MODIFIED_BY** 
[`integer`](../../../data-types.md) | Modified by ||
|| **DATE_START** 
[`string`](../../../data-types.md) | Start date ||
|| **DATE_END** 
[`string`](../../../data-types.md) | End date ||
|| **STATUS** 
[`string`](../../../data-types.md) | Status ||
|| **INFO** 
[`object`](../../../data-types.md) | Information ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "filter": {
        "GROUP_ID": 1,
        ">=DATE_END": "2024-07-19T15:03:01+00:00"
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.list
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "filter": {
        "GROUP_ID": 1,
        ">=DATE_END": "2024-07-19T15:03:01+00:00"
    },
    "auth": "YOUR_ACCESS_TOKEN"
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.list
    ```

- JS

    ```js
    const groupId = 1;
    BX24.callMethod(
        'tasks.api.scrum.sprint.list',
        {
            filter: {
                GROUP_ID: groupId,
                '>=DATE_END': new Date()
            }
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php'); // include CRest PHP SDK

    // execute request to REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.list',
        [
            'filter' => [
                'GROUP_ID' => 1,
                '>=DATE_END' => '2024-07-19T15:03:01+00:00'
            ]
        ]
    );

    // Process the response from Bitrix24
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
[
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
    },
    {
        "id": 3,
        "groupId": 1,
        "entityType": "sprint",
        "name": "Sprint 1",
        "goal": "",
        "sort": 1,
        "createdBy": 1,
        "modifiedBy": 1,
        "dateStart": "2021-11-21T22:00:00+00:00",
        "dateEnd": "2021-11-28T22:00:00+00:00",
        "status": "planned"
    }
]
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`object`](../../../data-types.md) | An object containing data about the sprint ||
|| **id** 
[`integer`](../../../data-types.md) | Sprint identifier ||
|| **groupId** 
[`integer`](../../../data-types.md) | Identifier of the group (Scrum) to which the sprint belongs ||
|| **entityType** 
[`string`](../../../data-types.md) | Entity type (in this case `sprint`) ||
|| **name** 
[`string`](../../../data-types.md) | Sprint name ||
|| **goal** 
[`string`](../../../data-types.md) | Sprint goal. Set only in the interface when starting the sprint ||
|| **sort** 
[`integer`](../../../data-types.md) | Sorting ||
|| **createdBy** 
[`integer`](../../../data-types.md) | Identifier of the user who created the sprint ||
|| **modifiedBy** 
[`integer`](../../../data-types.md) | Identifier of the user who modified the sprint ||
|| **dateStart** 
[`string`](../../../data-types.md) | Start date of the sprint in `ISO 8601` format ||
|| **dateEnd** 
[`string`](../../../data-types.md) | End date of the sprint in `ISO 8601` format ||
|| **status** 
[`string`](../../../data-types.md) | Sprint status ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Could not load list"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `0` | `Could not load list`| No sprints found with the specified filters ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-sprint-add.md)
- [{#T}](./tasks-api-scrum-sprint-update.md)
- [{#T}](./tasks-api-scrum-sprint-start.md)
- [{#T}](./tasks-api-scrum-sprint-complete.md)
- [{#T}](./tasks-api-scrum-sprint-get.md)
- [{#T}](./tasks-api-scrum-sprint-delete.md)
- [{#T}](./tasks-api-scrum-sprint-get-fields.md)