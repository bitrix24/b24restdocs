# Get a list of users with search by personal data user.search

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `user.search` method allows you to retrieve a list of users with accelerated search by personal data (first name, last name, middle name, department name, job title). It operates in two modes: quickly using **Fulltext Index** and a slower option through [right LIKE](*key_right LIKE) (support is determined automatically).

{% note info "" %}

The list of user fields in Bitrix24 that will be obtained as a result of executing the method depends on the application's/webhook's scope. Details about accessing user data can be found in [the article](index.md).

{% endnote %}

The method inherits the behavior of the [user.get](./user-get.md) method, and all parameters from this function are also available.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FILTER**
[`string`](../data-types.md) | The array can contain fields in any combination:
- `NAME` — first name
- `LAST_NAME` — last name
- `WORK_POSITION` — job title
- `UF_DEPARTMENT_NAME` — department name
- `USER_TYPE` — user type. Can take the following values: 
    - `employee` — employee
    - `extranet` — extranet user
    - `email` — email user

Or `FIND` — the field that will search in all listed fields.

{% note info "" %}

The method can work either with filtering using the FIND key or with all other fields. You cannot use FIND and any other field simultaneously.

{% endnote %} ||
|| **sort**
[`string`](../data-types.md) | The field by which the results are sorted. Sorting works on all fields from [user.add](./user-add.md) ||
|| **order**
[`string`](../data-types.md) | Sorting direction:
- `ASC` — ascending
- `DESC` — descending ||
|| **ADMIN_MODE**
[`boolean`](../data-types.md) | [Key for operation](*key_Key for operation) in administrator mode. Used to obtain data about any users ||
|| **start**
[`integer`](../data-types.md) | The parameter is used to manage pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "UF_DEPARTMENT": 1,
        "SORT": "ID",
        "ORDER": "asc",
        "start": 10
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.search
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "UF_DEPARTMENT": 1,
        "SORT": "ID",
        "ORDER": "asc",
        "start": 10,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.search
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'user.search',
    		{
    			'UF_DEPARTMENT': 1,
    			'SORT': 'ID',
    			'ORDER': 'asc',
    			'start': 10
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'user.search',
                [
                    'UF_DEPARTMENT' => 1,
                    'SORT'         => 'ID',
                    'ORDER'        => 'asc',
                    'start'        => 10,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error searching for users: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "user.search",
        {
            "UF_DEPARTMENT": 1,
            "SORT": "ID",
            "ORDER": "asc",
            "start": 10
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.search',
        [
            "UF_DEPARTMENT" => 1,
            "SORT" => 'ID',
            "ORDER" => 'asc',
            "start" => 10
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
                "ID": "1",
                "ACTIVE": true,
                "NAME": "Vadim",
                "LAST_NAME": "Valeev",
                "SECOND_NAME": "",
                "EMAIL": "v.r.valeev@bitrix.com",
                "LAST_LOGIN": "2024-07-25T13:06:54+00:00",
                "DATE_REGISTER": "2024-07-15T00:00:00+00:00",
                "TIME_ZONE": "",
                "IS_ONLINE": "Y",
                "TIMESTAMP_X": {
                },
                "LAST_ACTIVITY_DATE": {
                },
                "PERSONAL_GENDER": "",
                "PERSONAL_WWW": "",
                "PERSONAL_BIRTHDAY": "2018-07-14T00:00:00+00:00",
                "PERSONAL_MOBILE": "",
                "PERSONAL_CITY": "",
                "WORK_PHONE": "",
                "WORK_POSITION": "",
                "UF_EMPLOYMENT_DATE": "",
                "UF_DEPARTMENT": [1],
                "USER_TYPE": "employee"
            },
            {
                "ID": "3",
                "ACTIVE": true,
                "NAME": "Ivan",
                "LAST_NAME": "Ivanov",
                "EMAIL": "test@gmail.com",
                "LAST_LOGIN": "2024-07-24T09:01:55+00:00",
                "DATE_REGISTER": "2024-07-22T00:00:00+00:00",
                "IS_ONLINE": "N",
                "TIMESTAMP_X": {
                },
                "LAST_ACTIVITY_DATE": {
                },
                "PERSONAL_GENDER": "",
                "PERSONAL_BIRTHDAY": "",
                "WORK_POSITION": "",
                "UF_EMPLOYMENT_DATE": "",
                "UF_DEPARTMENT": [1],
                "USER_TYPE": "employee"
            }
        ],
        "total": 2,
        "time": {
            "start": 1721913235.39648,
            "finish": 1721913235.45078,
            "duration": 0.05430006980896,
            "processing": 0.0187909603118897,
            "date_start": "2024-07-25T13:13:55+00:00",
            "date_finish": "2024-07-25T13:13:55+00:00",
            "operating": 0
        }
    }
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response that contains the filtered list of users ||
|| **total**
[`integer`](../data-types.md) | The total number of records found ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-add.md)
- [{#T}](./user-update.md)
- [{#T}](./user-get.md)
- [{#T}](./user-current.md)
- [{#T}](./user-fields.md)

[*key_right LIKE]: USER_NAME LIKE "Text%" - this is called right like, when the search is performed only on text that starts with the specified phrase but can contain different endings - this search is significantly faster than two-way like "%text%" or left-sided "%text" - due to the architecture of storing indexed fields in the database.