# Key Provisions

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

Access permissions are available starting from version 18.7.0.

Permissions can only be granted by the account administrator, and only if the project plan is paid (in the case of the cloud). Currently, permissions can only be granted for sites, and they apply to all nested pages. For example, the permission "Change settings," granted for a site, applies to the site's settings page and the settings pages of any of its subpages.

{% note warning %}

If the account transitions from a paid category to a free one, the permission functionality is automatically disabled, and all restricted entities become accessible to everyone.

{% endnote %}

#| 
|| **Method** | **Description** ||
|| [landing.role.enable](./landing-role-enable.md) | Switches models ||
|| [landing.role.isEnabled](./landing-role-is-enabled.md) | Determines permission models ||
|#

## Extended Permission Model

#| 
|| **Method** | **Description** ||
|| [landing.site.getRights](./extended-model/landing-site-get-rights.md) | This method returns the permissions of the current user. ||
|| [landing.site.setRights](./extended-model/landing-site-set-rights.md) | Sets access permissions for the site. ||
|#

## Role-Based Permission Model

#| 
|| **Method** | **Description** ||
|| [landing.role.getList](./role-model/landing-role-get-list.md) | This method allows you to get a list of roles. ||
|| [landing.role.getRights](./role-model/landing-role-get-rights.md) | This method allows you to get a list of sites for which permissions are set within the role. ||
|| [landing.role.setAccessCodes](./role-model/landing-role-set-access-codes.md) | This method sets access codes for the role, which will be applicable to this role. ||
|| [landing.role.setRights](./role-model/landing-role-set-rights.md) | This method sets the necessary permissions within the role for site lists. ||
|#