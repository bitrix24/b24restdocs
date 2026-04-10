# Role-Based Access Model: Overview of Methods

The role-based access model helps manage access to sites in Bitrix24 through roles. A role combines permissions with those to whom they are assigned: users, groups, and departments. This approach eliminates the need to configure access for each user individually—permissions are set once and applied immediately to everyone assigned to the role.

For example, you can create a role called "Editor" and grant permission to edit website pages. Then, assign this role to the employees of a department. This way, all employees will receive the same permissions.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Site and Store Permissions](https://helpdesk.bitrix24.com/open/9375089/)

## How to Assign Roles and Configure Access Permissions

The configuration process consists of five main steps:

1. Check if the role-based access model is enabled. You can do this using the method [landing.role.isEnabled](../landing-role-is-enabled.md). If the model is disabled, enable it using the method [landing.role.enable](../landing-role-enable.md).

2. Determine the type of site for which roles are needed. If necessary, refer to the page [Site Types and Scopes](../../types.md).

3. Retrieve the list of available roles using the method [landing.role.getList](./landing-role-get-list.md) and select the desired role by `id`. After that, assign the role to users, groups, or departments using the method [landing.role.setAccessCodes](./landing-role-set-access-codes.md).

4. Configure the role's permissions for sites using the method [landing.role.setRights](./landing-role-set-rights.md). Permissions can be set for all sites at once or only for specific ones. To do this, use the `rights` parameter: key `0` sets default permissions for all sites, while key `<siteId>` sets permissions for a specific site.

5. After configuration, verify the result using the method [landing.role.getRights](./landing-role-get-rights.md).

## Relationship with Other Objects

The role-based access model works in conjunction with sites, access codes, and the permissions management mode in Bitrix24.

**Sites.** Role permissions are configured for specific sites, so a site identifier is required for operation. This can be obtained using the methods [landing.site.getList](../../site/landing-site-get-list.md) and [landing.site.add](../../site/landing-site-add.md).

**Access Codes.** Roles are assigned to users, groups, and departments through the `codes` parameter of the method [landing.role.setAccessCodes](./landing-role-set-access-codes.md). Examples of access codes and rules for their use can be found in the description of [landing.site.setRights](../extended-model/landing-site-set-rights.md).

**Permission Models.** Bitrix24 has both a role-based and an [extended permission model](../extended-model/index.md). Only one of them can be used at a time, so before configuration, check which model is currently enabled. This can be done using the method [landing.role.isEnabled](../landing-role-is-enabled.md).

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the methods: administrator or user with "full access" permission to the "Sites and Stores" section

#| 
|| **Method** | **Description** ||
|| [landing.role.getList](./landing-role-get-list.md) | Returns a list of roles for the selected type of sites ||
|| [landing.role.getRights](./landing-role-get-rights.md) | Returns the permissions of the specified role for each site for which they are configured ||
|| [landing.role.setAccessCodes](./landing-role-set-access-codes.md) | Specifies to whom the role is assigned: users, groups, or departments ||
|| [landing.role.setRights](./landing-role-set-rights.md) | Saves the default permissions of the role and for specific sites ||
|#