# Managing Company Cards: Overview of Methods

The group of methods `crm.company.details.configuration.*` manages the settings of the card for two views:
* "General view" — the card view for all employees
* "My view" — personal card settings for the employee

In the view, you can configure the sections of the card, for example, create a section for "Contact Information." Within the section, you can set the list of fields — in the "Contact Information" section, you can display the fields "Phone" and "Email." Fields that are not related to contact information should be displayed in another section.

> Quick navigation: [all methods](#all-methods) 

## Connection of Company Cards with Other Objects

**User.** The user identifier `userId` is used when setting personal card settings. You can obtain the user identifier using the method [user.get](../../../user/user-get.md).

**Company Fields.** The identifiers of the fields are used when setting visible fields in the card section. You can obtain the identifiers of system and custom company fields using the method [crm.company.fields](../crm-company-fields.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#| 
|| **Method** | **Description** ||
|| [crm.company.details.configuration.get](./crm-company-details-configuration-get.md) | Retrieves the settings of company cards ||
|| [crm.company.details.configuration.reset](./crm-company-details-configuration-reset.md) | Resets the settings of company cards ||
|| [crm.company.details.configuration.set](./crm-company-details-configuration-set.md) | Sets the settings of company cards ||
|| [crm.company.details.configuration.forceCommonScopeForAll](./crm-company-details-configuration-force-common-scope-for-all.md) | Allows forcing the common company card for all users || 
|#