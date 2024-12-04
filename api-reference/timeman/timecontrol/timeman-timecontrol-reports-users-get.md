# Get the list of users timeman.timecontrol.reports.users.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.timecontrol.reports.users.get` is used to retrieve a list of users belonging to the specified department.

## Parameters

#|
|| **Parameter** | **Example** | **Required** | **Description** ||
|| **DEPARTMENT_ID**
[`unknown`](../../data-types.md) | 52 | Yes* | Identifier of the department. ||
|#

*The `DEPARTMENT_ID` parameter must be specified only if the user is a manager or administrator.

## Example call

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'timeman.timecontrol.reports.users.get',
        {
            'DEPARTMENT_ID': 52
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    $result = restCommand(
        'timeman.timecontrol.reports.users.get',
        Array(
            'DEPARTMENT_ID' => 52
        ),
        $_REQUEST["auth"]
    );    
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in case of success

> 200 OK
```json
{
    "result": [
        {
            "id":2,
            "name":"Maria Ivshina",
            "first_name":"Maria",
            "last_name":"Ivshina",
            "work_position":"IT Specialist",
            "avatar":"http://test.bitrix24.com/upload/resize_cache/main/072/100_100_2/42-17948709.gif",
            "last_activity_date":"2018-08-15T16:25:34+02:00"
        }
    ]
}
```

### Description of keys

- **id** - user identifier.
- **name** - user's full name.
- **first_name** - user's first name.
- **last_name** - user's last name.
- **work_position** - job title.
- **avatar** - link to the avatar (if empty, the avatar is not set).
- **personal_gender** - user's gender.
- **last_activity_date** - date of the user's last action in ATOM format.