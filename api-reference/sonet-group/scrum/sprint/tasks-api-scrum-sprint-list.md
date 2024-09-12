# Get a list of sprints tasks.api.scrum.sprint.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
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

The method `tasks.api.scrum.sprint.list` returns a list of sprints.

This method is similar to other methods with filtering by list.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the result. The object format is `{'sorting_field': 'sorting_direction' [, ...]}`. Available fields are described in the table below.
Sorting direction can take the following values:
- `asc` - ascending;
- `desc` - descending. ||
  || **filter**
  [`object`](../../../data-types.md) | An object in the format `{'filter_field': 'filter_value' [, ...]}`. Available fields are described in the table below. ||
  || **select**
  [`object`](../../../data-types.md) | An array of record fields that will be returned by the method. You can specify only the fields you need. If the array contains the value `"*"`, all available fields will be returned.
  The default value - an empty array `array()` - means that all fields of the main query table will be returned. ||
  || **start**
  [`integer`](../../../data-types.md) | The page number of the output. Works for https requests. ||
|#

## Available filter fields

#|
|| **Field** | **Type** | **Description** ||
|| **ID** | `integer` | Sprint identifier ||
|| **GROUP_ID** | `integer` | Scrum identifier ||
|| **ENTITY_TYPE** | `string` | Entity type ||
|| **NAME** | `string` | Name ||
|| **SORT** | `integer` | Sorting ||
|| **CREATED_BY** | `integer` | Created by ||
|| **MODIFIED_BY** | `integer` | Modified by ||
|| **DATE_START** | `string` | Start date ||
|| **DATE_END** | `string` | End date ||
|| **STATUS** | `string` | Status ||
|| **INFO** | `object` | Information ||
|#

## Examples
{% list tabs %}

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

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.list',
        [
            'filter' => [
                'GROUP_ID' => 1,
                '>=DATE_END' => '2024-07-19T15:03:01+00:00'
            ]
        ]
    );

    // Processing the response from Bitrix24
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

## Returned data

#|
|| **Field** `type` | **Description** ||
|| **result** `object` | An object containing data about the sprint ||
|| **id** `integer` | Sprint identifier ||
|| **groupId** `integer` | Group (scrum) identifier to which the sprint belongs ||
|| **entityType** `string` | Entity type (in this case, "sprint") ||
|| **name** `string` | Sprint name ||
|| **goal** `string` | Sprint goal (set only in the interface when starting the sprint) ||
|| **sort** `integer` | Sorting ||
|| **createdBy** `integer` | Identifier of the user who created the sprint ||
|| **modifiedBy** `integer` | Identifier of the user who modified the sprint ||
|| **dateStart** `string` | Sprint start date in ISO 8601 format ||
|| **dateEnd** `string` | Sprint end date in ISO 8601 format ||
|| **status** `string` | Sprint status ||
|#

## Error handling

HTTP status: **200**

```json
{
  "error": 0,
  "error_description": "Could not load list"
}
```

### Possible error codes

#|
|| **Code** | **Description**  | **Value** ||
|| `0` | Could not load list| No sprints found with the specified filters ||
|#