# Application Settings: Overview of Methods

Application settings store general integration parameters and personal values for each user.

> Quick navigation: [all methods](#all-methods)

{% note info "" %}

The methods in this section work only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}

## How to Choose a Group of Methods

If the setting is the same for the entire application, use the method group `app.option.*`. This is suitable for general integration parameters that do not depend on the user.

If each user should have their own values, use the method group `user.option.*`. For example, for personal interface settings or operational parameters.

## Considerations Before Calling Methods

**Access Permissions.** The method [app.option.set](./app-option-set.md) is available only to administrators. The methods for reading settings and `user.option.*` are available to any user in the context of the application.

**User Binding.** The methods [user.option.set](./user-option-set.md) and [user.option.get](./user-option-get.md) save and read not the general application settings, but the settings of a specific user. Use them when different users should have different values, such as for personal interface settings.

## Connection with Other Objects

**Application.** The group of methods [System Methods](../system/index.md) checks the application context, available scopes, and the presence of the method on the account. For example, [app.info](../system/app-info.md) retrieves information about the application, while [scope](../system/scope.md) returns a list of available scopes.

**User.** The method [user.admin](../users/user-admin.md) checks the permissions of the current user before saving settings. The basic data of the current user is obtained through [profile](../users/profile.md).

## Overview of Methods    {#all-methods}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Application Settings

#| 
|| **Method** | **Description** ||
|| [app.option.set](./app-option-set.md) | Saves general application settings ||
|| [app.option.get](./app-option-get.md) | Retrieves general application settings ||
|#

### Current User Settings

#| 
|| **Method** | **Description** ||
|| [user.option.set](./user-option-set.md) | Saves the current user's settings for the application ||
|| [user.option.get](./user-option-get.md) | Retrieves the current user's settings for the application ||
|#