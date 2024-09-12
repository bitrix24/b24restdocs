# General Methods and Events

Several general methods available to the application regardless of the permissions requested by the application:

#|
|| **Method** | **Description** ||
|| [scope](./system/scope.md) | Displays permissions. ||
|| [app.info](./system/app-info.md) | Displays application information. ||
|| [access.name](./system/access-name.md) | Retrieves the names of access permissions. ||
|| [server.time](./system/server-time.md) | Returns the current server time in ATOM format. ||
|| [feature.get](./system/feature-get.md) | Checks the availability of features on the account. ||
|| [method.get](./system/method-get.md) | Checks the availability of a specific method. ||
|#

## Methods Related to Current User Permissions

#|
|| **Method** | **Description** ||
|| [user.admin](./users/user-admin.md) | Determines if the current user has permissions to manage application settings. ||
|| [user.access](./users/user-access.md) | Determines if the current user has at least one of the specified *ACCESS* permissions. ||
|#

## Storing and Retrieving Application Settings

#|
|| **Method** | **Description** ||
|| [app.option.set](./settings/app-option-set.md) | Set your application settings to be stored in Bitrix24. ||
|| [app.option.get](./settings/app-option-get.md) | Retrieve saved application settings from Bitrix24. ||
|| [user.option](./settings/user-option.md) | Set your current user settings to be stored in Bitrix24. ||
|| [user.option-set](./settings/user-option-set.md) | Retrieve saved settings of the current user from Bitrix24. ||
|#

## System Events

#|
|| **Event** | **Description** ||
|| [OnAppInstall](./events/on-app-install.md) | Triggered after the application is successfully installed on a specific Bitrix24 account. ||
|| [OnAppUninstall](./events/on-app-uninstall.md) | Triggered after the application is removed from a specific Bitrix24 account. ||
|| [OnAppMethodConfirm](./events/on-app-method-confirm.md) | Triggered after the portal administrator allows the use of a special class of methods. ||
|| [onAppPayment](./events/on-app-payment.md) | Triggered when the application is paid for. ||
|| [onAppUserAdd](./events/on-user-add.md) | Triggered when a user is added to Bitrix24. ||
|#