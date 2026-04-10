# Extended Rights Model: Overview of Methods

The extended rights model allows for configuring access separately for each site. In this model, rights are set in the site settings—based on access permissions for employees, groups, and departments according to their access codes.

For example, you can grant the marketing department the right to edit the promotions site, while all other employees can only view it.

> Quick Navigation: [all methods](#all-methods)

## How to Configure Rights in the Extended Model

The configuration process consists of five steps.

1. Check the current rights model using the method [landing.role.isEnabled](../landing-role-is-enabled.md). If the role model is enabled, switch to the extended model using the method [landing.role.enable](../landing-role-enable.md) with the value `mode: 0`.
2. Retrieve the site ID using the method [landing.site.getList](../../site/landing-site-get-list.md).
3. Prepare the `rights` object from access codes and operation codes.
4. Save the new set of rights using the method [landing.site.setRights](./landing-site-set-rights.md).
5. After configuration, verify the result under the appropriate user or in the Bitrix24 interface. The method [landing.site.getRights](./landing-site-get-rights.md) shows only the rights of the current user and does not return the complete list of rules saved for the site.

## Connection with Other Objects

The extended rights model is associated with sites, access codes, and the selected rights model in Bitrix24.

**Sites.** Rights are configured for a specific site, so the `siteId` is required. It can be obtained using the methods [landing.site.getList](../../site/landing-site-get-list.md) and [landing.site.add](../../site/landing-site-add.md).

**Access Codes.** Rights are assigned to users, groups, and departments through the `rights` object.

**Rights Models.** Only one rights model can operate at a time in Bitrix24. If access needs to be configured for a site, use the extended model. If the same rights need to be granted through roles, the [role-based rights model](../role-model/index.md) is suitable.

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#| 
|| **Method** | **Description** ||
|| [landing.site.getRights](./landing-site-get-rights.md) | Retrieves the current user's rights for the site ||
|| [landing.site.setRights](./landing-site-set-rights.md) | Sets access rights for the site ||
|#