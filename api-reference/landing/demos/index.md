# Custom Templates: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom templates allow you to add your own templates to the site and page creation wizard.

The group of methods `landing.demos.*` helps manage custom templates. This section includes methods for registering, retrieving lists, and deleting templates. Separate methods are used for registered and file templates.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to prepare a custom template](./introduction.md)

## Getting Started

1. Prepare a site or page using the methods from the Sites section [Sites section](../site/index.md)
2. Export the site using the [landing.site.fullExport](../site/landing-site-full-export.md) method
3. Save the exported array on the Bitrix24 application side
4. Pass the exported array to [landing.demos.register](./landing-demos-register.md)
5. Check the result using the [landing.demos.getList](./landing-demos-get-list.md) method

## Key Parameters

**XML_ID.** The external code of the registered template. It is used when deleting a template via [landing.demos.unregister](./landing-demos-unregister.md). You can obtain it using the [landing.demos.getList](./landing-demos-get-list.md) method. Deletion by code may affect related records of the template if they are registered in the application with the same code.

**type.** The type of template. It is specified during registration and when calling the [landing.demos.getSiteList](./landing-demos-get-site-list.md) and [landing.demos.getPageList](./landing-demos-get-page-list.md) methods. For example: `page`, `store`, `knowledge`.

**tpl_type.** The type of template usage in the wizard. This parameter is used when registering the template. There are two possible values: `S` — site template, `P` — page template.

## Relationship with Other Objects

**Site.** A custom template is created based on an existing site or page using the methods from the [Sites section](../site/index.md). To do this, the site is first exported using the [landing.site.fullExport](../site/landing-site-full-export.md) method. Then, the exported array is passed to [landing.demos.register](./landing-demos-register.md).

**Localization.** The title, description, and string values in the supported fields of the template can be localized during registration. This is done using the `lang` and `lang_original` parameters. More details can be found in the article [Template Localization](./localization.md).

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with View permission in the Sites section

#| 
|| **Method** | **Description** ||
|| [landing.demos.register](./landing-demos-register.md) | Registers a template in the site and page creation wizard ||
|| [landing.demos.getList](./landing-demos-get-list.md) | Retrieves a list of registered templates ||
|| [landing.demos.getSiteList](./landing-demos-get-site-list.md) | Retrieves a list of templates for site creation ||
|| [landing.demos.getPageList](./landing-demos-get-page-list.md) | Retrieves a list of templates for page creation ||
|| [landing.demos.unregister](./landing-demos-unregister.md) | Deletes a registered custom template ||
|#