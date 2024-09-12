# Get a List of Users by Filter user.get

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `user.get` method allows you to retrieve a filtered list of users. The method returns all users except for: bots, email users, users for Open Lines, and Replica users.

{% note info "" %}

The method does not return integrators. The list of fields for Bitrix24 users that will be obtained as a result of executing the method depends on the application's/webhook's scope. Details about user data access can be found in the [article](index.md).

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sort**
[`string`](../data-types.md) | The field by which the results are sorted. Sorting works on all fields from [user.add](./user-add.md) ||
|| **order**
[`string`](../data-types.md) | Sorting direction:
- `ASC` — ascending
- `DESC` — descending ||
|| **FILTER**
[`string`](../data-types.md) | You can additionally specify any parameters from [user.add](./user-add.md) to filter by their values. In addition to the main fields, the following additional fields are available:
- `UF_DEPARTMENT` — department affiliation;
- `UF_PHONE_INNER` — internal phone number;
- `IS_ONLINE` — [Y\|N] allows you to show only authorized users or not.
- `NAME_SEARCH` — quick search by personal data.
- `USER_TYPE` — user type. Can take the following values: 
    - `employee` — employee, 
    - `extranet` — extranet user, 
    - `email` — email user
- `ACTIVE` — when set to *true*, excludes terminated users from the request.
  
Filtering parameters can accept array values ||
|| **ADMIN_MODE**
[`boolean`](../data-types.md) | [Key for operation](*key_Key for operation) in administrator mode. Used to obtain data about any users ||
|| **start**
[`integer`](../data-types.md) | This parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.get
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
    https://**put_your_bitrix24_address**/rest/user.get
    ```

- JS

    ```js
    BX24.callMethod(
        "user.get",
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.get',
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

HTTP Status: **200**

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
                "TIME_ZONE_OFFSET": "7200",
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
                "NAME": "John",
                "LAST_NAME": "Doe",
                "EMAIL": "test@gmail.com",
                "LAST_LOGIN": "2024-07-24T09:01:55+00:00",
                "DATE_REGISTER": "2024-07-22T00:00:00+00:00",
                "IS_ONLINE": "N",
                "TIME_ZONE_OFFSET": "7200",
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
- [{#T}](./user-current.md)
- [{#T}](./user-search.md)
- [{#T}](./user-fields.md)

[*key_Key for operation]: `'ADMIN_MODE': 'True'`