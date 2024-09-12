# Methods for Working with Users in Bitrix24

> Scope: [`user`](../scopes/permissions.md)
>
> Who can perform the method: depending on the method

The methods for working with users in Bitrix24 allow inviting new users, modifying existing user data, and selecting users based on conditions. Applications that utilize these methods in their scenarios must ensure maximum security of user data and only retrieve the information about users that is truly necessary for the application's functionality.

To guarantee the security of users' personal information, there are several levels of access through user management methods:

- **Limited access versions**:
    - `user_brief`, which allows obtaining basic information about users without their contact details and custom fields. This scope is necessary and sufficient for scenarios where displaying the user's full name in the application interface is required.
    - `user_basic`, which allows obtaining not only basic information but also the contact details of Bitrix24 users. This scope is needed for scenarios related to making calls or sending e-mail messages through your application.

- **Full access versions**:
    - `user`, which allows access to all standard fields and also enables inviting new users and modifying existing user data.
    - `user.userfield`, which provides access to methods for working with user custom fields (expands the list of available fields in the reading methods available in the scopes above) for retrieving, adding, modifying, and deleting custom fields.

{% note info "Attention!" %}

This is the maximum level of access to personal information, and it should be requested very responsibly.

{% endnote %}

{% note info "Attention!" %}

The username length must not exceed 25 characters.

{% endnote %}

## Limited Versions of the User Scope

In these scopes, you cannot add/update users: the methods [user.add](./user-add.md) and [user.update](./user-update.md) are not available. In all other methods for obtaining user information, only these fields are accessible (starting from version **Rest 21.600.0**):

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
| WORK_PHONE | TIME_ZONE_OFFSET |
| WORK_POSITION | TIMESTAMP_X |
| WORK_COMPANY | DATE_REGISTER |
| IS_ONLINE | PERSONAL_PROFESSION |
| TIME_ZONE | PERSONAL_GENDER |
| TIMESTAMP_X | PERSONAL_BIRTHDAY |
| TIME_ZONE_OFFSET | PERSONAL_PHOTO |
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
| UF_FACEBOOK* | |
| UF_LINKEDIN | |
| UF_XING | |
| UF_WEB_SITES | |
| UF_PHONE_INNER | |
| UF_EMPLOYMENT_DATE | |
| UF_TIMEMAN | |
| UF_SKILLS | |
| UF_INTERESTS | |

{% note info "" %}

*The social network is recognized as extremist and is banned in the territory of the United States.

{% endnote %}

## Full Version of the User Scope

In the full version, the following fields are available (starting from version **Rest 21.600.0**):

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
|| TIME_ZONE_OFFSET ||
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
|| UF_FACEBOOK* ||
|| UF_LINKEDIN ||
|| UF_XING ||
|| UF_WEB_SITES ||
|| UF_PHONE_INNER ||
|| UF_EMPLOYMENT_DATE ||
|| UF_TIMEMAN ||
|| UF_SKILLS ||
|| UF_INTERESTS ||
|#

{% note info "" %}

*The social network is recognized as extremist and is banned in the territory of the United States.

{% endnote %}

## Methods

#|
|| **Method** | **Description** ||
|| [user.fields](user-fields.md) | Retrieve a list of user field names ||
|| [user.current](user-current.md) | Get information about the current user ||
|| [user.add](user-add.md) | Invite a user ||
|| [user.update](user-update.md) | Update user data ||
|| [user.get](user-get.md) | Retrieve a filtered list of users ||
|| [user.search](user-search.md) | Get a list of users with accelerated search based on personal data ||
|#