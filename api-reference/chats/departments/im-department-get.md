# Get Information About the Department im.department.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.department.get` retrieves data about a department.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | `[51]` | Department identifiers | 18 ||
|| **USER_DATA**
[`unknown`](../../data-types.md) | `N` | Load user data | 18 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

- If the parameter `USER_DATA = Y` is passed, the manager's data will be included in the result.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.department.get',
        {
            ID: [51]
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

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.department.get',
        Array(
            'ID' => [51],
        ),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

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
    ]
}    
```

### Key Descriptions

- `id` – department identifier
- `name` – short name of the department
- `full_name` – full name of the department
- `manager_user_data` – object describing the manager's data (not available if `USER_DATA != 'Y'`):
- `id` – user identifier
- `name` – user's first and last name
- `first_name` – user's first name
- `last_name` – user's last name
- `work_position` – position
- `color` – user's color in hex format
- `avatar` – link to the avatar (if empty, the avatar is not set)
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
    "error": "INVALID_FORMAT",
    "error_description": "A wrong format for the ID field is passed"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **INVALID_FORMAT** | An incorrect format for identifiers was passed ||
|#