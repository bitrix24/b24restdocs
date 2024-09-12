# Get the list of participants im.dialog.users.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.dialog.users.list` retrieves information about chat participants. Pagination is supported.

## Parameters

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `chat74` | Identifier of the dialog. Format:
- **chatXXX** – chat of the recipient, if the message is for a chat
- **XXX** – identifier of the recipient, if the message is for a private dialog | 30 ||
|| **SKIP_EXTERNAL**
[`unknown`](../../data-types.md) | `N` | Skip all system users - `'Y'`\|`'N'` (default `'N'`) | 30 ||
|| **SKIP_EXTERNAL_EXCEPT_TYPES**
[`unknown`](../../data-types.md) | `'bot, email'` | A string with the types of system users to include in the selection | 30 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

```js
B24.callMethod(
    'im.dialog.users.list',
    {
        DIALOG_ID: 'chat74',
        SKIP_EXTERNAL: 'Y'
    },
    res => {
        if (res.error())
        {
        console.error(result.error().ex);
        }
        else
        {
        console.log(res.data())
        }
    }
)
```

{% include [Example Notes](../../../_includes/examples.md) %}

## Successful Response

```json
[
    {
        "id": 1019,
        "active": true,
        "name": "alexa shasha",
        "first_name": "alexa",
        "last_name": "shasha",
        "work_position": "",
        "color": "#df532d",
        "avatar": "",
        "gender": "M",
        "birthday": "",
        "extranet": false,
        "network": false,
        "bot": false,
        "connector": false,
        "external_auth_id": "default",
        "status": "online",
        "idle": false,
        "last_activity_date": "2021-10-30T11:24:12+02:00",
        "mobile_last_date": "2021-10-20T13:02:33+02:00",
        "absent": false,
        "departments": [
        1
        ],
        "phones": false
    },
    {
        "id": 1,
        "active": true,
        "name": "Alex Smith",
        "first_name": "Alex",
        "last_name": "Smith",
        "work_position": "",
        "color": "#df532d",
        "avatar": "",
        "gender": "M",
        "birthday": "",
        "extranet": false,
        "network": false,
        "bot": false,
        "connector": false,
        "external_auth_id": "default",
        "status": "online",
        "idle": false,
        "last_activity_date": "2021-10-30T13:36:34+02:00",
        "mobile_last_date": "2021-10-27T16:39:26+02:00",
        "absent": false,
        "departments": [
        1
        ],
        "phones": false
    }
]
```

### Key Descriptions

- `id` – user identifier
- `active` – whether the user is active (not terminated)
- `name` – user's full name
- `first_name` – user's first name
- `last_name` – user's last name
- `work_position` – job title
- `color` – user's color in **hex** format
- `avatar` – link to the avatar (if empty, the avatar is not set)
- `gender` – user's gender
- `birthday` – user's birthday in **DD-MM** format (if empty, not set)
- `extranet` – indicator of external extranet user (`true/false`)
- `network` – indicator of **Bitrix24.Network** user (`true/false`)
- `bot` – indicator of bot (`true/false`)
- `connector` – indicator of open lines user (`true/false`)
- `external_auth_id` – external authorization code
- `status` – selected user status
- `idle` – date when the user stepped away from the computer, in **ATOM** format (if not set, then `false`)
- `last_activity_date` – date of the user's last action in **ATOM** format
- `mobile_last_date` – date of the last action in the mobile app in **ATOM** format (if not set, then `false`)
- `departments` – identifiers of the department
- `absent` – date until which the user is on vacation, in **ATOM** format (if not set, then `false`)
- `phones` – array of phone numbers:
  - `work_phone` – work phone
  - `personal_mobile` – mobile phone
  - `personal_phone` – home phone

## Error Response

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **DIALOG_ID_EMPTY** | The `DIALOG_ID` parameter is not set or does not match the format ||
|| **ACCESS_ERROR** | The current user does not have access permissions to the data ||
|#