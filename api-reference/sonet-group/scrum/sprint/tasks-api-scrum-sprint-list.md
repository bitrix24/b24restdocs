# Get the list of sprints tasks.api.scrum.sprint.list

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.sprint.list` returns a list of sprints.

This method is similar to other methods with filtering by list.

## Method parameters

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the result. The object format is `{'sorting_field': 'sorting_direction' [, ...]}`. Available fields are described in the table [below](#fields).

Sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending ||
|| **filter**
[`object`](../../../data-types.md) | An object in the format `{'filter_field': 'filter_value' [, ...]}`. Available fields are described in the table [below](#fields) ||
|| **select**
[`object`](../../../data-types.md) | An array of record fields that will be returned by the method. You can specify only the fields that are necessary. 

If the array contains the value `"*"`, all available fields will be returned.

The default value is an empty array `array()`. In this case, all fields of the main query table will be returned ||
|| **start**
[`integer`](../../../data-types.md) | The page number of the output. Works for https requests ||
|#

### Available filter fields {#fields}

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

## Code examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

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
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    const groupId = 1;
    try {
      const response = await $b24.callListMethod(
        'tasks.api.scrum.sprint.list',
        {
          filter: {
            GROUP_ID: groupId,
            '>=DATE_END': new Date()
          }
        },
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    const groupId = 1;
    try {
      const generator = $b24.fetchListMethod('tasks.api.scrum.sprint.list', {
        filter: {
          GROUP_ID: groupId,
          '>=DATE_END': new Date()
        }
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    const groupId = 1;
    try {
      const response = await $b24.callMethod('tasks.api.scrum.sprint.list', {
        filter: {
          GROUP_ID: groupId,
          '>=DATE_END': new Date()
        }
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    $groupId = 1;
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.sprint.list',
                [
                    'filter' => [
                        'GROUP_ID'    => $groupId,
                        '>=DATE_END' => new DateTime(),
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching sprint list: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

    ```php
    require_once('crest.php'); // include CRest PHP SDK

    // execute a request to the REST API
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

## Response handling

HTTP status: **200**

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

### Returned data

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
[`string`](../../../data-types.md) | Name of the sprint ||
|| **goal** 
[`string`](../../../data-types.md) | Goal of the sprint. Set only in the interface when starting the sprint ||
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
[`string`](../../../data-types.md) | Status of the sprint ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "Could not load list"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Error message** | **Description** ||
|| `0` | `Could not load list`| No sprints found with the specified filters ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue exploring

- [{#T}](./tasks-api-scrum-sprint-add.md)
- [{#T}](./tasks-api-scrum-sprint-update.md)
- [{#T}](./tasks-api-scrum-sprint-start.md)
- [{#T}](./tasks-api-scrum-sprint-complete.md)
- [{#T}](./tasks-api-scrum-sprint-get.md)
- [{#T}](./tasks-api-scrum-sprint-delete.md)
- [{#T}](./tasks-api-scrum-sprint-get-fields.md)

