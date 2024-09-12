# Overview of Methods

Two objects are used to work with application settings:
- `BX24.userOption` — works with the settings of the current user. This object operates after [BX24.init](../system-functions/bx24-init.md). The value of user settings is set immediately.
- `BX24.appOption` — works with the general settings of the application. This object operates after [BX24.init](../system-functions/bx24-init.md). Setting application settings is available only to users with application management access permission (see [BX24.isAdmin](../additional-functions/bx24-is-admin.md)). A completion handler may be required for application settings (the `callback` parameter in [BX24.appOption.set](./bx24-app-option-set.md)).

#|
|| **Method** | **Description** ||
|| [BX24.userOption.set](./bx24-user-option-set.md) | This method sets the settings for the current user ||
|| [BX24.userOption.get](./bx24-user-option-get.md) | This method returns the settings for the current user ||
|| [BX24.appOption.set](./bx24-app-option-set.md) | This method sets the general settings for the current application ||
|| [BX24.appOption.get](./bx24-app-option-get.md) | This method returns the setting by its code ||
|#