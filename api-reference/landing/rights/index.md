# Access Rights in Websites and Stores: Overview of Methods

In Bitrix24, access to websites and stores can be managed using both extended and role-based access models. These methods allow you to check which model is currently enabled, switch between them if necessary, and configure access permissions as needed.

If you need to set access rights specifically for a particular website, the [extended model](./extended-model/index.md) is suitable. In this model, access is defined at the website level. For example, you can allow a group of employees to edit one website while restricting others to view-only access.

If you need to grant the same rights to multiple users or a department, the [role-based model](./role-model/index.md) is appropriate. In this case, you first retrieve a list of roles, assign a role to employees, and then configure the role's access rights for the websites. For instance, you can create a role called "Editor," assign it to a department, and allow them to modify the company's websites.

The settings for the extended and role-based models are stored separately. When switching between them, previously assigned rights are not deleted. When the model is re-enabled, its settings take effect again.

{% note warning %}

Access rights are set for the website and apply to all its pages. In the cloud version of Bitrix24, this setting is only available on paid plans. If you switch to a free plan, the access rights configuration will be automatically disabled, and all previously restricted objects will become accessible to all users.

{% endnote %}

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Access Rights for Websites and Stores](https://helpdesk.bitrix24.com/open/9375089/)

## How to Configure the Access Rights Model

1. Check which access rights model is currently enabled using the [landing.role.isEnabled](./landing-role-is-enabled.md) method.
2. If necessary, switch the model using the [landing.role.enable](./landing-role-enable.md) method.
3. Retrieve the website ID using [landing.site.getList](../site/landing-site-get-list.md).
4. For the extended model, configure the website's access rights in the [Extended Model](./extended-model/index.md) section.
5. For the role-based model, obtain the list of roles, assign access codes, and save the rights in the [Role-Based Model](./role-model/index.md) section.
6. Verify the result: for the extended model, call [landing.site.getRights](./extended-model/landing-site-get-rights.md); for the role-based model, call [landing.role.getRights](./role-model/landing-role-get-rights.md).

## Relationship with Other Objects

**Websites.** Access rights are configured for websites, so a `siteId` is required. This can be obtained using the [landing.site.getList](../site/landing-site-get-list.md) and [landing.site.add](../site/landing-site-add.md) methods.

**Pages.** The access rights for the website apply to all its pages, so there is no need to configure access separately for pages.

**Access Codes and Roles.** In the role-based model, access is not assigned directly to users but through roles and access codes. This is done using the [landing.role.setAccessCodes](./role-model/landing-role-set-access-codes.md) and [landing.role.setRights](./role-model/landing-role-set-rights.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

### Switching the Access Rights Model

#| 
|| **Method** | **Description** ||
|| [landing.role.enable](./landing-role-enable.md) | Enables or disables the role-based access model ||
|| [landing.role.isEnabled](./landing-role-is-enabled.md) | Checks if the role-based access model is enabled ||
|#

### Extended Model

#| 
|| **Method** | **Description** ||
|| [landing.site.getRights](./extended-model/landing-site-get-rights.md) | Retrieves the current user's rights for the website ||
|| [landing.site.setRights](./extended-model/landing-site-set-rights.md) | Sets access rights for the website ||
|#

### Role-Based Model

#| 
|| **Method** | **Description** ||
|| [landing.role.getList](./role-model/landing-role-get-list.md) | Retrieves the list of roles for the current type of websites ||
|| [landing.role.getRights](./role-model/landing-role-get-rights.md) | Returns the rights of the role by websites ||
|| [landing.role.setAccessCodes](./role-model/landing-role-set-access-codes.md) | Sets access codes for the role ||
|| [landing.role.setRights](./role-model/landing-role-set-rights.md) | Sets the rights of the role by websites ||
|#