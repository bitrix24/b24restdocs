# CRM Form Placement and Channels: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

You can show a ready-made CRM form to visitors in several ways: embed it in a website page, open it when a button is clicked, show it automatically after a set delay, or send the customer a public link. The `crm.form.embedding.*` methods return the code and link for these options and save the display settings. The `crm.form.sitebutton.*` and `crm.form.openline.*` methods show which website widgets and Open Channels already use the form.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Place CRM Form on Bitrix24 Site](https://helpdesk.bitrix24.com/open/25364920/)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

## Form Placement Options

- `inline` — the form is embedded in a website page. This option has no display settings
- `click` — the form opens when a button or link is clicked. You can configure the window view and position, as well as the Bitrix24 button: text, font, alignment, and colors
- `auto` — the form opens automatically. You can configure the window view and position and the display delay in seconds
- `link` — a public link to the form. You can send it to the customer by email or in a messenger

{% note tip "User Documentation" %}

- [Place CRM Form on Bitrix24 Site](https://helpdesk.bitrix24.com/open/25364920/)
- [Place a CRM Form on a Non-Bitrix24 Site](https://helpdesk.bitrix24.com/open/25654730/)

{% endnote %}

## Working with Placement

1. Retrieve the form identifier with the [crm.form.list](../crm-form-list.md) method. To accept submissions, the form must be active: you can activate it with the [crm.form.activate](../crm-form-activate.md) method
2. Retrieve all placement options with the [crm.form.embedding.get](./crm-form-embedding-get.md) method. The response contains the code for each option, the public link, the current display settings, and their allowed values
3. To change the on-click display or auto-display, retrieve the form's `updatedTime` with the [crm.form.get](../crm-form-get.md) method and pass the new settings to [crm.form.embedding.update](./crm-form-embedding-update.md)
4. Insert the code of the required option into a website page or send the customer the public link. To retrieve only one option, pass the `variant` parameter to [crm.form.embedding.get](./crm-form-embedding-get.md). For the `click` and `auto` options, the same request can return code with a different button text or delay without saving the settings
5. Check where the form is already used: in website widgets with the [crm.form.sitebutton.list](./crm-form-sitebutton-list.md) method, in Open Channels with the [crm.form.openline.list](./crm-form-openline-list.md) method

## Method Specifics

**Form version.** The [crm.form.embedding.update](./crm-form-embedding-update.md) method accepts the required `updatedTime` parameter — the form version. If the form was changed after you retrieved the version, the method returns the `CRM_FORM_VERSION_MISMATCH` error. Saving placement settings changes the form version, so before the next call to [crm.form.update](../crm-form-update.md) or [crm.form.embedding.update](./crm-form-embedding-update.md), retrieve the new `updatedTime` value.

**The code on the website updates automatically.** After the settings are saved, Bitrix24 rebuilds the form files. You do not need to change code that is already on the website.

**Linking to widgets and Open Channels.** The [crm.form.sitebutton.list](./crm-form-sitebutton-list.md) and [crm.form.openline.list](./crm-form-openline-list.md) methods only read data. You cannot connect a form to a website widget or set it as the welcome form of an Open Channel via REST — this is done in the Bitrix24 interface.

**A form filter is required.** The widget and Open Channel list methods accept exactly one filter condition `[["formId", "=", <form_id>]]`, sorting only by `id`, and pagination via `pagination`.

## Limitations

- Placement settings can be changed by a user with permission to edit CRM forms. System forms cannot be changed
- The widget list requires permission to view website widgets, and the Open Channel list requires permission to edit Open Channels and the installed Open Channels module. The [crm.form.openline.list](./crm-form-openline-list.md) method returns only the channels the user can edit
- An Open Channel can have only one welcome form. If another form occupies the channel, its identifier is returned in the `welcomeFormId` field
- If Bitrix24 requires phone number verification to work with CRM forms, the methods return an error until the number is verified
- On plans where editing CRM forms is unavailable, [crm.form.embedding.update](./crm-form-embedding-update.md) returns the `CRM_FORM_WRITE_RESTRICTED` error

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Placement Options

#|
|| **Method** | **Description** ||
|| [crm.form.embedding.get](./crm-form-embedding-get.md) | Returns the code and link for placing the form, the current display settings, and their allowed values ||
|| [crm.form.embedding.update](./crm-form-embedding-update.md) | Saves the on-click display and auto-display settings of the form ||
|| [crm.form.embedding.field.list](./crm-form-embedding-field-list.md) | Returns a list of placement option fields ||
|| [crm.form.embedding.field.get](./crm-form-embedding-field-get.md) | Returns the description of a placement option field ||
|#

### Website Widgets

#|
|| **Method** | **Description** ||
|| [crm.form.sitebutton.list](./crm-form-sitebutton-list.md) | Returns website widgets and shows whether the form is connected to them ||
|| [crm.form.sitebutton.field.list](./crm-form-sitebutton-field-list.md) | Returns a list of website widget fields ||
|| [crm.form.sitebutton.field.get](./crm-form-sitebutton-field-get.md) | Returns the description of a website widget field ||
|#

### Open Channels

#|
|| **Method** | **Description** ||
|| [crm.form.openline.list](./crm-form-openline-list.md) | Returns Open Channels and shows whether the form is their welcome form ||
|| [crm.form.openline.field.list](./crm-form-openline-field-list.md) | Returns a list of Open Channel fields ||
|| [crm.form.openline.field.get](./crm-form-openline-field-get.md) | Returns the description of an Open Channel field ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../crm-form-get.md)
- [{#T}](../data-types.md)
