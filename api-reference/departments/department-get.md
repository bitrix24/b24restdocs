# Get the list of departments department.get

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `department.get` method is designed to retrieve a list of departments.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sort**
[`string`](../data-types.md) | The name of the field to sort by. Sorting works on the following fields: 
- `ID` — department identifier 
- `NAME` — department name
- `SORT` — sorting order
- `PARENT` — parent department identifier
- `UF_HEAD` — identifier of the department head ||
|| **order**
[`string`](../data-types.md) | Sorting direction:
- `ASC` — ascending
- `DESC` — descending ||
|| **ID**
[`int`](../data-types.md) | Filter by department identifier ||
|| **NAME**
[`string`](../data-types.md) | Filter by department name. The full name of the department must be specified ||
|| **SORT**
[`int`](../data-types.md) | Filter by sorting field ||
|| **PARENT**
[`int`](../data-types.md) | Filter by parent department identifier ||
|| **UF_HEAD**
[`int`](../data-types.md) | Filter by department head identifier ||
|| **START**
[`integer`](../data-types.md) | This parameter is used to manage pagination.

The page size for results is always static: 50 records.

To select the second page of results, you must pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number.

Details in the article [{#T}](../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "sort": "NAME",
        "order": "DESC",
        "PARENT": 1
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/department.get
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "sort": "NAME",
        "order": "DESC",
        "PARENT": 1,
        "auth":"**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/department.get
    ```

- JS

    ```js
    BX24.callMethod(
        "department.get", {
            "sort": "NAME",
            "order": "DESC",
            "PARENT": 1
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'department.get',
        [
            'sort' => 'NAME',
            'order' => 'DESC',
            'PARENT' => 1,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
  "result": [
    {
      "ID": "15",
      "NAME": "R&D",
      "SORT": 500,
      "PARENT": "1"
    },
    {
      "ID": "2",
      "NAME": "Accounting",
      "SORT": 500,
      "PARENT": "1"
    },
    {
      "ID": "4",
      "NAME": "Marketing and Advertising Department",
      "SORT": 500,
      "PARENT": "1"
    },
    {
      "ID": "3",
      "NAME": "Sales Department",
      "SORT": 500,
      "PARENT": "1"
    },
    {
      "ID": "17",
      "NAME": "Legal Department",
      "SORT": 500,
      "PARENT": "1"
    }
  ],
  "total": 5,
  "time": {
    "start": 1736924749.531017,
    "finish": 1736924749.719845,
    "duration": 0.1888279914855957,
    "processing": 0.00179290771484375,
    "date_start": "2025-01-15T07:05:49+00:00",
    "date_finish": "2025-01-15T07:05:49+00:00",
    "operating": 0
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`department[]`](#st_department) | The root element of the response that contains the filtered list of departments ||
|| **total**
[`integer`](../data-types.md) | The total number of records found ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

### Structure of department {#st_department}

```json
    {
        "ID": "2",
        "NAME": "Accounting",
        "SORT": 500,
        "PARENT": "1"
    }
```

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`int`](../data-types.md) | Department identifier ||
|| **NAME**
[`string`](../data-types.md) | Department name ||
|| **SORT**
[`int`](../data-types.md) | Department sorting field ||
|| **PARENT**
[`int`](../data-types.md) | Parent department identifier ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./department-add.md)
- [{#T}](./department-update.md)
- [{#T}](./department-delete.md)
- [{#T}](./department-fields.md)