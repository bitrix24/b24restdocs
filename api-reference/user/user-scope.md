# User Scope Versions

To ensure the security of employee data, different versions of the `User` scope are available for applications and webhooks with the [module version](../../settings/cloud-and-on-premise/on-premise/versions.md) **Rest 21.600.0**.

- `user_brief` provides access to user information without contact details. This is sufficient for scenarios where displaying the user's full name in a third-party application's interface is required.
- `user_basic` opens basic information and contact details of users. This is necessary for scenarios related to making calls or sending e-mails.
- `user` grants full access to user information, the ability to invite new users, and modify existing user data.

To access custom fields, add the `user.userfield` scope to the application.

## Limited user scope versions

In these scopes, adding and updating users is not allowed: the methods [user.add](./user-add.md) and [user.update](./user-update.md) are not available. In all other methods for retrieving user information, only the listed fields are accessible.

| user_basic | user_brief |
|------------|------------|
| ID | ID |
| XML_ID | XML_ID |
| ACTIVE | ACTIVE |
| NAME | NAME |
| LAST_NAME | LAST_NAME |
| SECOND_NAME | SECOND_NAME |
| TITLE | TITLE |
| EMAIL | IS_ONLINE |
| PERSONAL_PHONE | TIME_ZONE |
| WORK_PHONE | PERSONAL_PHOTO |
| WORK_POSITION | TIMESTAMP_X |
| WORK_COMPANY | DATE_REGISTER |
| IS_ONLINE | PERSONAL_PROFESSION |
| TIME_ZONE | PERSONAL_GENDER |
| TIMESTAMP_X | PERSONAL_BIRTHDAY |
| DATE_REGISTER | PERSONAL_CITY |
| LAST_ACTIVITY_DATE | PERSONAL_STATE |
| PERSONAL_PROFESSION | PERSONAL_COUNTRY |
| PERSONAL_GENDER | WORK_POSITION |
| PERSONAL_BIRTHDAY | WORK_CITY |
| PERSONAL_PHOTO | WORK_STATE |
| PERSONAL_PHONE | WORK_COUNTRY |
| PERSONAL_FAX | LAST_ACTIVITY_DATE |
| PERSONAL_MOBILE | UF_EMPLOYMENT_DATE |
| PERSONAL_PAGER | UF_TIMEMAN |
| PERSONAL_STREET | UF_SKILLS |
| PERSONAL_MAILBOX | UF_INTERESTS |
| PERSONAL_CITY | UF_DEPARTMENT |
| PERSONAL_STATE | UF_PHONE_INNER |
| PERSONAL_ZIP | |
| PERSONAL_COUNTRY | |
| PERSONAL_NOTES | |
| WORK_COMPANY | |
| WORK_DEPARTMENT | |
| WORK_POSITION | |
| WORK_WWW | |
| WORK_PHONE | |
| WORK_FAX | |
| WORK_PAGER | |
| WORK_STREET | |
| WORK_MAILBOX | |
| WORK_CITY | |
| WORK_STATE | |
| WORK_ZIP | |
| WORK_COUNTRY | |
| WORK_PROFILE | |
| WORK_LOGO | |
| WORK_NOTES | |
| UF_DEPARTMENT | |
| UF_DISTRICT | |
| UF_SKYPE | |
| UF_SKYPE_LINK | |
| UF_ZOOM | |
| UF_TWITTER | |
| UF_FACEBOOK | |
| UF_LINKEDIN | |
| UF_XING | |
| UF_WEB_SITES | |
| UF_PHONE_INNER | |
| UF_EMPLOYMENT_DATE | |
| UF_TIMEMAN | |
| UF_SKILLS | |
| UF_INTERESTS | |

## Full user scope version

{% note info " " %}

This is the highest level of access to personal information, and it should be requested very responsibly.

{% endnote %}

In the full version, all system fields are available, along with the ability to create and modify user profiles.

#|
|| **user** ||
|| ID ||
|| XML_ID ||
|| ACTIVE ||
|| NAME ||
|| LAST_NAME ||
|| SECOND_NAME ||
|| TITLE ||
|| EMAIL ||
|| LAST_LOGIN ||
|| DATE_REGISTER ||
|| TIME_ZONE ||
|| IS_ONLINE ||
|| TIMESTAMP_X ||
|| LAST_ACTIVITY_DATE ||
|| PERSONAL_PROFESSION ||
|| PERSONAL_GENDER ||
|| PERSONAL_WWW ||
|| PERSONAL_BIRTHDAY ||
|| PERSONAL_PHOTO ||
|| PERSONAL_ICQ ||
|| PERSONAL_PHONE ||
|| PERSONAL_FAX ||
|| PERSONAL_MOBILE ||
|| PERSONAL_PAGER ||
|| PERSONAL_STREET ||
|| PERSONAL_MAILBOX ||
|| PERSONAL_CITY ||
|| PERSONAL_STATE ||
|| PERSONAL_ZIP ||
|| PERSONAL_COUNTRY ||
|| PERSONAL_NOTES ||
|| WORK_COMPANY ||
|| WORK_DEPARTMENT ||
|| WORK_POSITION ||
|| WORK_WWW ||
|| WORK_PHONE ||
|| WORK_FAX ||
|| WORK_PAGER ||
|| WORK_STREET ||
|| WORK_MAILBOX ||
|| WORK_CITY ||
|| WORK_STATE ||
|| WORK_ZIP ||
|| WORK_COUNTRY ||
|| WORK_PROFILE ||
|| WORK_LOGO ||
|| WORK_NOTES ||
|| UF_DEPARTMENT ||
|| UF_DISTRICT ||
|| UF_SKYPE ||
|| UF_SKYPE_LINK ||
|| UF_ZOOM ||
|| UF_TWITTER ||
|| UF_FACEBOOK ||
|| UF_LINKEDIN ||
|| UF_XING ||
|| UF_WEB_SITES ||
|| UF_PHONE_INNER ||
|| UF_EMPLOYMENT_DATE ||
|| UF_TIMEMAN ||
|| UF_SKILLS ||
|| UF_INTERESTS ||
|#