# User Scope Versions

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The `user` scope comes in three versions. These are three independent codes: `user_brief`, `user_basic`, and `user`. The version determines which employee profile fields the methods return. It also determines whether an application can invite employees and modify their profiles. Versions let you grant an application only the personal data its scenario requires.

All three versions are available in cloud Bitrix24. In on-premise Bitrix24, they were introduced in the [module version](../../settings/cloud-and-on-premise/on-premise/versions.md) `Rest 21.600.0`. Before that version, only the `user` scope is available.

Scopes are selected in the settings of an application or a webhook — the procedure is described in the [Available Scopes in Bitrix24](../scopes/permissions.md) article. For profile field values and the response format, see the [user.get](./user-get.md) and [user.fields](./user-fields.md) method pages.

## How the Versions Differ

| Scope | Name in the Permission List | Access |
|---|---|---|
| `user_brief` | Users (minimum) | Read access to 30 fields: name, position, photo, city, date of birth, internal number |
| `user_basic` | Users (basic) | Read access to 60 fields: employee contacts — e-mail, phone numbers, addresses, and links to external profiles |
| `user` | Users | Read access to 63 fields, inviting employees, and modifying profiles |

The versions are nested: `user_basic` includes all `user_brief` fields, and `user` includes all `user_basic` fields.

The minimum version does not hide all personal data. In `user_brief`, contact details are closed first of all: e-mail, personal and work phone numbers, street and postal code, links to external profiles. At the same time, gender, date of birth, and the employee photo are available in all three versions. The full composition of each version is in the tables below.

The `user` version adds three fields to `user_basic`: `LAST_LOGIN`, `PERSONAL_WWW`, and `PERSONAL_ICQ`. Its main difference lies not in the fields, but in write operations.

## How to Select a Version

Request the narrowest version that is sufficient for your scenario: the application receives less personal data, and the Bitrix24 administrator sees a lower access level in the permission list.

| Application Scenario | Version |
|---|---|
| Display an employee name, photo, or position in the interface | `user_brief` |
| Find an employee by name, position, or department | `user_brief` |
| Call an employee or send them an e-mail | `user_basic` |
| Pass employee contacts to an external system | `user_basic` |
| Invite employees or update profiles from an HR system | `user` |
| Retrieve the date of an employee's last authorization | `user` |

{% note info "" %}

The `user` version is the highest level of access to employee personal data.

{% endnote %}

## How the Version Restriction Works

The restriction applies to every call of the methods that read or modify a profile: [user.fields](./user-fields.md), [user.current](./user-current.md), [user.get](./user-get.md), [user.search](./user-search.md), [user.add](./user-add.md), and [user.update](./user-update.md).

- Each of these methods returns and accepts only the fields permitted by the version.
- The `user.online` and `user.counters` methods are available in all three versions: they do not return profile fields.
- Bitrix24 skips a field that is not in the permitted list. Such a field is ignored in the `select` and `filter` parameters, it is absent from the response, and the method does not return an error.
- The `user.add` and `user.update` methods work only in the `user` version. In the `user_brief` and `user_basic` versions, a call returns the `insufficient_scope` error with the description `The request requires higher privileges than provided by the access token`. The response is identical for an application and a webhook.
- The scope does not cancel the employee permission check. New employees can be invited with the `user.add` method by an administrator, and in cloud Bitrix24 also by an employee who has been granted the invitation permission. Without administrator permissions, the `user.update` method modifies only the employee's own profile — the `ACTIVE` and `UF_DEPARTMENT` fields are not retained in this case.
- An application receives the [onUserAdd](../common/events/on-user-add.md) event in all three versions. Only the fields permitted by the version remain in the new employee data.
- The broadest of the granted versions determines the access. For example, if `user_brief` and `user_basic` are granted, `user_basic` applies.
- The selected version does not affect the [user.userfield.*](./userfields/index.md) methods: they manage custom field configurations and work in the separate `user.userfield` scope.

To see the actual list of fields for the granted version, call the [user.fields](./user-fields.md) method.

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.fields
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/user.fields
    ```

{% endlist %}

The method returns field codes and their names — only those permitted by the granted version. The composition of the response identifies the version:

- the `EMAIL` field is absent — `user_brief`
- `EMAIL` is present, but `LAST_LOGIN` is absent — `user_basic`
- `LAST_LOGIN` is present — `user`

### Custom Profile Fields

Fields with the `UF_USR_` prefix are not included in the version lists.

- In the `user` version, they are available for reading and writing without additional conditions.
- In the `user_brief` and `user_basic` versions, they can only be read, provided that the application has been granted the `user.userfield` scope. A value cannot be retained in such a field: this requires the `user.update` method, which works only in the `user` version.

Such fields are created and configured with the [user.userfield.*](./userfields/index.md) methods.

## Which Fields Are Available

The tables list the standard profile fields, including the fields with the `UF_` prefix that exist in Bitrix24 by default. `Yes` means the field is available for reading in this version, `No` means it is unavailable. Fields are retained only by the `user.add` and `user.update` methods, so in the `user_brief` and `user_basic` versions any field is available for reading only.

The `LAST_LOGIN`, `DATE_REGISTER`, and `IS_ONLINE` fields are available for reading only. The `user.add` and `user.update` methods do not retain them even in the `user` version.

### Identification and Status

| Field | `user_brief` | `user_basic` | `user` |
|---|---|---|---|
| `ID` | Yes | Yes | Yes |
| `XML_ID` | Yes | Yes | Yes |
| `ACTIVE` | Yes | Yes | Yes |
| `USER_TYPE` | Yes | Yes | Yes |
| `IS_ONLINE` | Yes | Yes | Yes |
| `TIME_ZONE` | Yes | Yes | Yes |
| `DATE_REGISTER` | Yes | Yes | Yes |
| `TIMESTAMP_X` | Yes | Yes | Yes |
| `LAST_ACTIVITY_DATE` | Yes | Yes | Yes |
| `LAST_LOGIN` | No | No | Yes |

### Name, Position, Department

| Field | `user_brief` | `user_basic` | `user` |
|---|---|---|---|
| `NAME` | Yes | Yes | Yes |
| `LAST_NAME` | Yes | Yes | Yes |
| `SECOND_NAME` | Yes | Yes | Yes |
| `TITLE` | Yes | Yes | Yes |
| `WORK_POSITION` | Yes | Yes | Yes |
| `UF_DEPARTMENT` | Yes | Yes | Yes |
| `WORK_COMPANY` | No | Yes | Yes |
| `WORK_DEPARTMENT` | No | Yes | Yes |

### Contacts

| Field | `user_brief` | `user_basic` | `user` |
|---|---|---|---|
| `UF_PHONE_INNER` | Yes | Yes | Yes |
| `EMAIL` | No | Yes | Yes |
| `PERSONAL_PHONE` | No | Yes | Yes |
| `PERSONAL_MOBILE` | No | Yes | Yes |
| `PERSONAL_FAX` | No | Yes | Yes |
| `PERSONAL_PAGER` | No | Yes | Yes |
| `PERSONAL_MAILBOX` | No | Yes | Yes |
| `WORK_PHONE` | No | Yes | Yes |
| `WORK_FAX` | No | Yes | Yes |
| `WORK_PAGER` | No | Yes | Yes |
| `WORK_MAILBOX` | No | Yes | Yes |
| `WORK_WWW` | No | Yes | Yes |
| `UF_SKYPE` | No | Yes | Yes |
| `UF_SKYPE_LINK` | No | Yes | Yes |
| `UF_ZOOM` | No | Yes | Yes |
| `UF_TWITTER` | No | Yes | Yes |
| `UF_FACEBOOK` | No | Yes | Yes |
| `UF_LINKEDIN` | No | Yes | Yes |
| `UF_XING` | No | Yes | Yes |
| `UF_WEB_SITES` | No | Yes | Yes |
| `PERSONAL_WWW` | No | No | Yes |
| `PERSONAL_ICQ` | No | No | Yes |

### Addresses

| Field | `user_brief` | `user_basic` | `user` |
|---|---|---|---|
| `PERSONAL_CITY` | Yes | Yes | Yes |
| `PERSONAL_STATE` | Yes | Yes | Yes |
| `PERSONAL_COUNTRY` | Yes | Yes | Yes |
| `WORK_CITY` | Yes | Yes | Yes |
| `WORK_STATE` | Yes | Yes | Yes |
| `WORK_COUNTRY` | Yes | Yes | Yes |
| `PERSONAL_STREET` | No | Yes | Yes |
| `PERSONAL_ZIP` | No | Yes | Yes |
| `WORK_STREET` | No | Yes | Yes |
| `WORK_ZIP` | No | Yes | Yes |
| `UF_DISTRICT` | No | Yes | Yes |

### Personal and Professional Data

| Field | `user_brief` | `user_basic` | `user` |
|---|---|---|---|
| `PERSONAL_PHOTO` | Yes | Yes | Yes |
| `PERSONAL_BIRTHDAY` | Yes | Yes | Yes |
| `PERSONAL_GENDER` | Yes | Yes | Yes |
| `PERSONAL_PROFESSION` | Yes | Yes | Yes |
| `UF_SKILLS` | Yes | Yes | Yes |
| `UF_INTERESTS` | Yes | Yes | Yes |
| `UF_EMPLOYMENT_DATE` | Yes | Yes | Yes |
| `UF_TIMEMAN` | Yes | Yes | Yes |
| `PERSONAL_NOTES` | No | Yes | Yes |
| `WORK_PROFILE` | No | Yes | Yes |
| `WORK_LOGO` | No | Yes | Yes |
| `WORK_NOTES` | No | Yes | Yes |

## Continue Learning

- [{#T}](../scopes/permissions.md)
- [{#T}](./user-fields.md)
- [{#T}](./user-get.md)
- [{#T}](./index.md)
