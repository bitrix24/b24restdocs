# How to Prepare a Custom Template

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A custom template is a ready-made blueprint for a website or page that can be added to the site creation wizard. The wizard is the screen where a Bitrix24 user selects a design when creating a new website or page. Your own template appears in this list once an application registers it.

The template is created based on an existing website or page from the [Sites section](../site/index.md). First, the website is exported — its structure is unloaded into a data set that can be saved and passed on. This data set is then registered using the [`landing.demos.*` methods](./index.md) from the Bitrix24 application.

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with View permission in the Sites section

## When to Use a Custom Template

Use a custom template if you need to:

- add your own template to the website creation wizard
- distribute a ready-made website or page through the application
- reuse the same set of pages, blocks, and settings

If the template only needs to be installed in one Bitrix24 without reuse, it is sufficient to create and configure the website or page in the usual way.

{% note tip "" %}

If the template is distributed as an application with a website, refer to the articles [Installing Website Templates](../../../settings/app-installation/site-templates-installation.md) and [Website Requirements Before Publishing](../../../market/preparing-to-publish/requirements-sites.md).

{% endnote %}

## How a Custom Template Works

A template is made up of three interconnected parts:

- **the source website or page** — the basis for the template, prepared in the [Sites section](../site/index.md)
- **the export** — the website structure as a data set, created by the [landing.site.fullExport](../site/landing-site-full-export.md) method
- **the registered template** — an entry that appears in the wizard after calling [landing.demos.register](./landing-demos-register.md)

The section's methods handle the individual steps of working with a template:

- [landing.demos.register](./landing-demos-register.md) — registers the template in the website and page creation wizard
- [landing.demos.getList](./landing-demos-get-list.md) — returns the registered templates and lets you verify the registration result. If the method is called from an application, the response includes only that application's templates
- [landing.demos.getSiteList](./landing-demos-get-site-list.md) — returns the website templates available in the wizard for the selected site type. The list includes both built-in Bitrix24 templates and matching templates you have registered. For example, for the `store` type, the method returns online store templates
- [landing.demos.getPageList](./landing-demos-get-page-list.md) — the same, but for individual page templates
- [landing.demos.unregister](./landing-demos-unregister.md) — deletes a registered template

## How to Prepare the Template

Before exporting, check the website itself:

- pages are correctly linked to each other
- the necessary blocks and themes are used
- images and external resources are accessible via working URLs
- title, description, and preview data are prepared for the template. Preview data is the preview images used to identify the template in the wizard's list

If the website is multi-page, use a single theme for all pages. This helps maintain a consistent appearance after the template is installed.

## How to Register the Template

The process is as follows:

1. Create a website or page that will serve as the basis for the template
2. Export the website using the [landing.site.fullExport](../site/landing-site-full-export.md) method — it returns the website structure as a data set
3. Save the export result on the application side
4. When installing the application, pass the saved data to [landing.demos.register](./landing-demos-register.md). Typically, the result of `fullExport` is passed without manually restructuring the data
5. Verify the result using the [landing.demos.getList](./landing-demos-get-list.md) method: the template should appear in the list and in the website or page creation wizard

What the methods return:

- [landing.demos.register](./landing-demos-register.md) — an array of numeric identifiers for the templates it created or updated
- [landing.demos.getList](./landing-demos-get-list.md) — a list of templates. Each one has an external code `XML_ID`, a title `TITLE`, a type `TYPE`, and other fields. You can narrow the selection using the `select`, `filter`, `order`, `limit`, and `offset` parameters

## What to Consider Before Registration

**Template Type.** The template data includes the `type` and `tpl_type` fields. The `type` field defines the template's purpose: `page` — pages, `store` — stores, `knowledge` — knowledge bases, `group` — groups, `mainpage` — home pages. The `tpl_type` field defines the template's location in the wizard: `S` — website template, `P` — page template. Check both fields in the structure you pass to [landing.demos.register](./landing-demos-register.md).

**Export Composition.** Usually, the complete result of [landing.site.fullExport](../site/landing-site-full-export.md) is passed for registration. If the structure is modified manually, make sure the required fields and the page map `items` are preserved. If the required `code` field with the template's external code is left empty, the method returns the `BX_EMPTY_REQUIRED` error.

**Security Check.** Before registration, Bitrix24 checks the template's content. If unsafe code is found, the method returns the `CONTENT_IS_BAD` error, and the template is not registered.

**Preview Data.** Prepare `preview`, `preview2x`, `preview3x`, and `preview_url` if the template should be displayed in the list and in the preview.

**External Template Code.** To delete a template, you need its external code. Get it as follows:

- during registration, the external code is set in the `code` field of the [landing.demos.register](./landing-demos-register.md) method — it is saved as the template's `XML_ID`
- if the code was not saved, you can retrieve it using the [landing.demos.getList](./landing-demos-get-list.md) method, in the `XML_ID` field
- pass this code to the `code` parameter of the [landing.demos.unregister](./landing-demos-unregister.md) method

If no template with this code exists, `unregister` returns `false`. If a website template and a page template are registered with the same code, deletion may affect both related records.

## Preview URL

`preview_url` specifies the preview page of the template in the wizard. This URL can be passed when exporting the website via [landing.site.fullExport](../site/landing-site-full-export.md). It is then used during template registration.

For `preview_url`, a published page that showcases the template in its final form is typically used. For a multi-page website, the main page is sufficient.

Ensure that the preview link remains accessible. Otherwise, the preview page will not open in the wizard.

## Images and External Resources

When exporting the website, images and other external resources may be saved as absolute links. After the template is installed, they will be loaded from the original address. This will continue until the user replaces them with their own files.

If the template is distributed to other Bitrix24 accounts, check in advance:

- that all URLs are accessible externally
- that images do not depend on temporary storage
- that preview images will not be deleted

## Template Localization

The title and description of the template can be localized during registration. For this, the `lang` and `lang_original` parameters are used in [landing.demos.register](./landing-demos-register.md).

If the template is to be displayed in Bitrix24 with different languages, prepare the localization array in advance. For details, see the article [Template Localization](./localization.md).

## Continue Learning

- [Overview of Custom Template Methods](./index.md)
- [Register a Template in the Website Creation Wizard](./landing-demos-register.md)
- [Get a List of Registered Templates](./landing-demos-get-list.md)
- [Get a List of Templates for Website Creation](./landing-demos-get-site-list.md)
- [Get a List of Templates for Page Creation](./landing-demos-get-page-list.md)
- [Delete a Registered Template](./landing-demos-unregister.md)
- [Export a Website](../site/landing-site-full-export.md)
- [Template Localization](./localization.md)
