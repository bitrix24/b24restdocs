# CRM Forms: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A CRM form collects customer data on a website, in a widget, or via a link and creates CRM objects from it: a lead, deal, contact, company, estimate, or SPA item.

The `crm.form.*` methods belong to [REST 3.0](../../rest-v3.md) and cover the full CRM form lifecycle:

- search for forms by exact name and by filters: activity, target CRM object, responsible person, creation date
- retrieve the complete form configuration
- retrieve the scenarios, target objects, pipelines, and fields of selected objects available in Bitrix24
- create a form with builder settings and validate the configuration before saving
- partially update a form without overwriting other settings
- activate and deactivate a form with the active forms limit taken into account
- delete a form with its state and the user's permissions taken into account
- retrieve the embed code and the public link, configure the button, position, and auto-display delay
- view the website widgets and Open Channels the form is connected to

The structure of all objects in this section and the rules for writing fields are described in [Data Types and Object Structure in the CRM Forms REST API](./data-types.md).

{% note warning "" %}

The methods are not available on the Free and trial plans.

{% endnote %}

> Quick navigation: [all methods](#all-methods)

{% note info "" %}

The methods are called at a URL containing `/rest/api/`. The PHP SDK, BX24.js, and PHP CRest do not support this URL yet — use direct HTTP requests or the JS SDK `@bitrix24/b24jssdk`

{% endnote %}

## Getting Started

1. Retrieve the builder directories with [crm.form.configuration.get](./setup/crm-form-configuration-get.md): CRM schemes `entitySchemes`, pipelines, field types, languages, payment systems, user permissions, and the active forms limit
2. Choose a form scenario with [crm.form.scenario.list](./setup/crm-form-scenario-list.md) if you want to create a form from a ready-made template
3. Retrieve the field codes of the objects in the selected scheme with [crm.form.entityfield.list](./setup/crm-form-entityfield-list.md). Take the object names from `entitySchemes[].entityNames`
4. Validate the configuration with [crm.form.validate](./crm-form-validate.md). The method returns all violations at once and saves nothing
5. Create the form with [crm.form.add](./crm-form-add.md). A new form is active by default; to create a deactivated form, pass `active: false`
6. Activate the form with [crm.form.activate](./crm-form-activate.md) if you created it deactivated
7. Retrieve the website code or the public link with [crm.form.embedding.get](./placement/crm-form-embedding-get.md)

{% note tip "User documentation" %}

- [Create and Configure CRM Forms](https://helpdesk.bitrix24.com/open/24977144/)
- [CRM Form Use Cases](https://helpdesk.bitrix24.com/open/25113886/)
- [List of CRM Forms](https://helpdesk.bitrix24.com/open/25311354/)
- [CRM Form Design](https://helpdesk.bitrix24.com/open/25268756/)

{% endnote %}

## Method Specifics

**Partial update.** [crm.form.update](./crm-form-update.md) changes only the passed fields. Collections, such as the form fields `fields` or the responsible persons queue `responsible.userIds`, are replaced entirely. The write format matches the read format — see [CRM Form Structure](./data-types.md#crm_form).

**Form version.** The `updatedTime` field is the form version. [crm.form.update](./crm-form-update.md) and [crm.form.embedding.update](./placement/crm-form-embedding-update.md) require the version retrieved with [crm.form.get](./crm-form-get.md). [crm.form.delete](./crm-form-delete.md), [crm.form.activate](./crm-form-activate.md), and [crm.form.deactivate](./crm-form-deactivate.md) accept it optionally. If the form has been changed in the meantime, the method returns the `CRM_FORM_VERSION_MISMATCH` error with the current version in `currentUpdatedTime`. [crm.form.embedding.update](./placement/crm-form-embedding-update.md) also changes the version: after updating the placement settings, retrieve the form again.

**Field selection.** Without `select`, [crm.form.get](./crm-form-get.md) and [crm.form.list](./crm-form-list.md) return only the simple form fields. Sections such as `crm`, `fields`, or `appearance` must be requested in `select`.

**Activity and deletion.** Only [crm.form.activate](./crm-form-activate.md) and [crm.form.deactivate](./crm-form-deactivate.md) change the activity. An active form can be deleted only with the `confirmed: true` confirmation. System forms with `system: true` cannot be updated, deactivated, or deleted.

A form is activated only if its configuration passes the [crm.form.validate](./crm-form-validate.md) check and the active forms limit is not reached. Any non-system form can be deactivated, even if it fails validation or the limit is reached. Activating an active form again or deactivating a deactivated one does not change its state. A deactivated form can be deleted without confirmation; deleting it again returns a "not found" error.

**One form per call.** The update methods work with one form by `id` and do not accept `filter`.

## Permissions and Limitations

**User permissions.** The methods use their own CRM forms permission model. Reading requires the permission to view forms; creating, updating, activating, and deleting require the permission to edit forms. [crm.form.configuration.get](./setup/crm-form-configuration-get.md) returns the current user's permissions in the `permissions` key.

**Plan.** The plan restrictions apply:

- the active forms limit. The current number and the maximum are in the `limits.activeForms` key of the [crm.form.configuration.get](./setup/crm-form-configuration-get.md) response. If the limit is exceeded, the methods return the `CRM_FORM_ACTIVATION_LIMIT_REACHED` error
- removing the "Powered by Bitrix24" label. If the plan does not allow it, the value `appearance.copyrightRemoved: true` is saved as `false`
- access to editing forms. If the plan does not allow editing forms, the write methods return the `CRM_FORM_WRITE_RESTRICTED` error

**Form fields.** The set of available fields depends on the CRM scheme `crm.entityScheme`: only fields of the objects in the selected scheme can be added to the form.

**Authorization method.** The methods work the same way via OAuth, via an inbound webhook with the `crm` scope, and when an agent works through [Alaio Vibecode](../../../ai-tools/vibecode.md).

{% note tip "User documentation" %}

- [Configure Access Permissions for CRM Forms](https://helpdesk.bitrix24.com/open/23883556/)

{% endnote %}

## Connection with Other Objects

A CRM form is linked to CRM objects, users, websites, and Open Channels.

**CRM objects.** Submitting a form creates objects according to the `crm.entityScheme` scheme. [crm.form.configuration.get](./setup/crm-form-configuration-get.md) returns the scheme identifiers, the deal pipelines `dealCategoryId`, and the SPA pipelines `smartProcessCategoryId`. [crm.form.entityfield.list](./setup/crm-form-entityfield-list.md) returns the object field codes for the `fields` array.

**Users.** The persons responsible for submissions are specified in `responsible.userIds`. User identifiers can be retrieved with [user.get](../../user/user-get.md).

**Websites and widgets.** [crm.form.embedding.get](./placement/crm-form-embedding-get.md) returns the code for embedding on a website and the public link. [crm.form.sitebutton.list](./placement/crm-form-sitebutton-list.md) returns the website widgets the form is connected to.

**Open Channels.** [crm.form.openline.list](./placement/crm-form-openline-list.md) returns Open Channels and indicates whether the form is the welcome form in them. A form cannot be linked to a widget or an Open Channel via REST: the methods only read data.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Forms

#|
|| **Method** | **Description** ||
|| [crm.form.add](./crm-form-add.md) | Creates a CRM form ||
|| [crm.form.update](./crm-form-update.md) | Partially updates a CRM form ||
|| [crm.form.get](./crm-form-get.md) | Returns a CRM form by identifier ||
|| [crm.form.list](./crm-form-list.md) | Returns a list of CRM forms with filtering and sorting ||
|| [crm.form.delete](./crm-form-delete.md) | Deletes a CRM form ||
|| [crm.form.validate](./crm-form-validate.md) | Validates a form configuration without saving ||
|| [crm.form.activate](./crm-form-activate.md) | Activates a CRM form ||
|| [crm.form.deactivate](./crm-form-deactivate.md) | Deactivates a CRM form ||
|| [crm.form.field.list](./crm-form-field-list.md) | Returns a list of CRM form fields ||
|| [crm.form.field.get](./crm-form-field-get.md) | Returns the description of a CRM form field by name ||
|#

### Form Setup Directories

The methods return the data required to create and update a form. Learn more in the [Form Setup Directories](./setup/index.md) overview.

#|
|| **Method** | **Description** ||
|| [crm.form.configuration.get](./setup/crm-form-configuration-get.md) | Returns the form builder directories: CRM schemes, pipelines, field types, languages, permissions, and limits ||
|| [crm.form.configuration.field.list](./setup/crm-form-configuration-field-list.md) | Returns a list of builder configuration fields ||
|| [crm.form.configuration.field.get](./setup/crm-form-configuration-field-get.md) | Returns the description of a builder configuration field by name ||
|| [crm.form.scenario.list](./setup/crm-form-scenario-list.md) | Returns form creation scenarios ||
|| [crm.form.scenario.field.list](./setup/crm-form-scenario-field-list.md) | Returns a list of scenario fields ||
|| [crm.form.scenario.field.get](./setup/crm-form-scenario-field-get.md) | Returns the description of a scenario field by name ||
|| [crm.form.scenariocategory.list](./setup/crm-form-scenariocategory-list.md) | Returns scenario categories ||
|| [crm.form.scenariocategory.field.list](./setup/crm-form-scenariocategory-field-list.md) | Returns a list of scenario category fields ||
|| [crm.form.scenariocategory.field.get](./setup/crm-form-scenariocategory-field-get.md) | Returns the description of a scenario category field by name ||
|| [crm.form.entityfield.list](./setup/crm-form-entityfield-list.md) | Returns the CRM object fields that can be added to a form ||
|| [crm.form.entityfield.field.list](./setup/crm-form-entityfield-field-list.md) | Returns a list of fields describing a CRM object field ||
|| [crm.form.entityfield.field.get](./setup/crm-form-entityfield-field-get.md) | Returns the description of a field from the crm.form.entityfield.list response by name ||
|#

### Placement and Channels

The methods return the website code, configure the button and auto-display, and show the form's widgets and Open Channels. Learn more in the [Placement and Channels](./placement/index.md) overview.

#|
|| **Method** | **Description** ||
|| [crm.form.embedding.update](./placement/crm-form-embedding-update.md) | Saves the settings for showing the form on click and for auto-display ||
|| [crm.form.embedding.get](./placement/crm-form-embedding-get.md) | Returns the code and link for placing the form, the current display settings, and their allowed values ||
|| [crm.form.embedding.field.list](./placement/crm-form-embedding-field-list.md) | Returns a list of placement option fields ||
|| [crm.form.embedding.field.get](./placement/crm-form-embedding-field-get.md) | Returns the description of a placement option field by name ||
|| [crm.form.sitebutton.list](./placement/crm-form-sitebutton-list.md) | Returns website widgets and shows whether the form is connected to them ||
|| [crm.form.sitebutton.field.list](./placement/crm-form-sitebutton-field-list.md) | Returns a list of website widget fields ||
|| [crm.form.sitebutton.field.get](./placement/crm-form-sitebutton-field-get.md) | Returns the description of a website widget field by name ||
|| [crm.form.openline.list](./placement/crm-form-openline-list.md) | Returns Open Channels and shows whether the form is the welcome form in them ||
|| [crm.form.openline.field.list](./placement/crm-form-openline-field-list.md) | Returns a list of Open Channel fields ||
|| [crm.form.openline.field.get](./placement/crm-form-openline-field-get.md) | Returns the description of an Open Channel field by name ||
|#
