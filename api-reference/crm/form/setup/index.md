# CRM Form Setup Directories: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The directories show what a CRM form can be built from in a specific Bitrix24 account: which CRM objects it can create, in which pipelines, from which fields, in which language, and with which payment systems. Values from the directories are passed to the form fields when calling [crm.form.add](../crm-form-add.md), [crm.form.update](../crm-form-update.md), and [crm.form.validate](../crm-form-validate.md).

The methods in this section let you:

- retrieve the schemes for creating CRM objects, deal and SPA pipelines, languages, form field types, payment systems, user permissions, and the active forms limit
- retrieve form scenarios and the categories they are grouped by
- retrieve the lead, contact, company, deal, estimate, or SPA fields that can be added to a form

The directories only read data and do not change anything in Bitrix24.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [CRM Form Use Cases](https://helpdesk.bitrix24.com/open/25113886/)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

## Usage Order When Creating a Form

1. Retrieve the builder configuration using the [crm.form.configuration.get](./crm-form-configuration-get.md) method. Check `permissions.canEdit` and the `limits.activeForms` limit, choose a scheme from `entitySchemes` and, if needed, a pipeline from `dealCategories` or `smartProcesses[].categories`
2. If the form is created from a ready-made template, choose a scenario using the [crm.form.scenario.list](./crm-form-scenario-list.md) method. To group scenarios, use [crm.form.scenariocategory.list](./crm-form-scenariocategory-list.md)
3. Retrieve the fields of the scheme objects using the [crm.form.entityfield.list](./crm-form-entityfield-list.md) method. Pass the codes from `entitySchemes[].entityNames` of the selected scheme to the filter
4. Build the form configuration and validate it using the [crm.form.validate](../crm-form-validate.md) method, then create the form using the [crm.form.add](../crm-form-add.md) method

## How Directory Values Get into a Form

#|
|| **Directory value** | **Form field** ||
|| `entitySchemes[].id` from [crm.form.configuration.get](./crm-form-configuration-get.md) | `crm.entityScheme` ||
|| `duplicateModes[].id` | `crm.duplicateMode` ||
|| `dealCategories[].id` | `crm.dealCategoryId` ||
|| `smartProcesses[].categories[].id` | `crm.smartProcessCategoryId` ||
|| `languages[].id` | `languageId` ||
|| `fieldTypes[].id` | `fields[].type` ||
|| `paySystems[].id` | `payment.disabledPaySystemsId` — payment systems the form does not offer ||
|| `name` from [crm.form.entityfield.list](./crm-form-entityfield-list.md) | `fields[].name` ||
|| `id` from [crm.form.scenario.list](./crm-form-scenario-list.md) | `templateId` ||
|#

## Section Limitations

- The contents of the directories depend on the Bitrix24 account: enabled leads, SPAs, pipelines, payment systems, and custom fields
- The [crm.form.entityfield.list](./crm-form-entityfield-list.md) method requires a filter by CRM objects; one request can return fields for no more than 10 objects
- Fields with options cannot yet be added to a form via REST: lists, checkboxes, radio buttons, as well as products and booking. Such fields are present in the `crm.form.entityfield.list` response, but the form write methods do not accept them
- The directory methods do not support filtering, sorting, or pagination, except for the filter by objects in `crm.form.entityfield.list`

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: a user with permission to view CRM forms. The `*.field.list` and `*.field.get` methods — any user

### Builder Configuration

#|
|| **Method** | **Description** ||
|| [crm.form.configuration.get](./crm-form-configuration-get.md) | Returns CRM object schemes, pipelines, languages, field types, payment systems, user permissions, and the active forms limit ||
|| [crm.form.configuration.field.list](./crm-form-configuration-field-list.md) | Returns a list of builder configuration fields ||
|| [crm.form.configuration.field.get](./crm-form-configuration-field-get.md) | Returns the description of a builder configuration field ||
|#

### Scenarios

#|
|| **Method** | **Description** ||
|| [crm.form.scenario.list](./crm-form-scenario-list.md) | Returns CRM form scenarios ||
|| [crm.form.scenario.field.list](./crm-form-scenario-field-list.md) | Returns a list of scenario fields ||
|| [crm.form.scenario.field.get](./crm-form-scenario-field-get.md) | Returns the description of a scenario field ||
|#

### Scenario Categories

#|
|| **Method** | **Description** ||
|| [crm.form.scenariocategory.list](./crm-form-scenariocategory-list.md) | Returns CRM form scenario categories ||
|| [crm.form.scenariocategory.field.list](./crm-form-scenariocategory-field-list.md) | Returns a list of scenario category fields ||
|| [crm.form.scenariocategory.field.get](./crm-form-scenariocategory-field-get.md) | Returns the description of a scenario category field ||
|#

### CRM Object Fields

#|
|| **Method** | **Description** ||
|| [crm.form.entityfield.list](./crm-form-entityfield-list.md) | Returns the CRM object fields a form can be built from ||
|| [crm.form.entityfield.field.list](./crm-form-entityfield-field-list.md) | Returns a list of fields describing a CRM object field ||
|| [crm.form.entityfield.field.get](./crm-form-entityfield-field-get.md) | Returns the description of a field from the CRM object field directory ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../crm-form-add.md)
- [{#T}](../crm-form-validate.md)
- [{#T}](../data-types.md)
