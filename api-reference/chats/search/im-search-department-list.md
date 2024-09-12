# Find Departments im.search.department.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.department.list` performs a search for departments.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **FIND^*^**
[`unknown`](../../data-types.md) | `Moscow` | Search phrase | 19 ||
|| **USER_DATA**
[`unknown`](../../data-types.md) | `N` | Load user data | 19 ||
|| **OFFSET**
[`unknown`](../../data-types.md) | `0` | Offset for user selection | 19 ||
|| **LIMIT**
[`unknown`](../../data-types.md) | `10` | Limit for user selection | 19 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

- If the parameter `USER_DATA = Y` is passed, data about the manager will be loaded into the result.
- The search is conducted on the following fields: **Full department name**.
- The method supports standard pagination of the Bitrix24 Rest API, but in addition, it allows navigation using the `OFFSET` and `LIMIT` parameters.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.search.department.list',
        {
            FIND: 'Moscow'
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log('users', result.data());
                console.log('total', result.total());
            }
        }
    );
    ```

- PHP

    ```php
    $result = restCommand(
        'im.search.department.list',
        Array(
            'FIND' => 'Moscow'
        ),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

{% endlist %}

{% include [Examples Notes](../../../_includes/examples.md) %}

## Successful Response

```json
{    
    "result": [
        {
            "id": 51,
            "name": "Moscow Branch",
            "full_name": "Moscow Branch / Bitrix",
            "manager_user_id": 11
        }
    ],
    "total": 1
}            
```

### Key Descriptions

- `id` – department identifier
- `name` – short department name
- `full_name` – full department name
- `manager_user_data` – object describing manager data (not available if `USER_DATA != 'Y'`)
- `id` – user identifier
- `name` – user's first and last name
- `first_name` – user's first name
- `last_name` – user's last name
- `work_position` – position
- `color` – user's color in hex format
- `avatar` – link to avatar (if empty, avatar is not set)
- `gender` – user's gender
- `birthday` – user's birthday in DD-MM format, if empty – not set
- `extranet` – indicator of external extranet user (`true/false`)
- `network` – indicator of Bitrix24.Network user (`true/false`)
- `bot` – indicator of bot (`true/false`)
- `connector` – indicator of open lines user (`true/false`)
- `external_auth_id` – external authorization code
- `status` – selected user status
- `idle` – date when the user stepped away from the computer, in ATOM format (if not set, `false`)
- `last_activity_date` – date of the user's last action in ATOM format
- `mobile_last_date` – date of the last action in the mobile app in ATOM format (if not set, `false`)
- `absent` – date until which the user is on vacation, in ATOM format (if not set, `false`)

## Error Response

```json
{
    "error": "FIND_SHORT",
    "error_description": "Too short a search phrase."
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **FIND_SHORT** | The search phrase is too short; the search requires at least three characters. ||
|#