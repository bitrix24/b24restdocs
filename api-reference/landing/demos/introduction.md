# How to Prepare a Custom Template

A custom template is created based on an existing website or page from the [Sites section](../site/index.md). The template is then exported and registered using the methods from the `landing.demos.*` section of the Bitrix24 application.

This page outlines the general workflow. It explains how to prepare the source website, how to obtain the export, and what to pass to the methods in the `landing.demos.*` section.

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

A custom template is created based on an existing website from the [Sites section](../site/index.md). To register the template, you need to:

1. Prepare the website and pages
2. Export the website using the [landing.site.fullExport](../site/landing-site-full-export.md) method
3. Pass the exported array to [landing.demos.register](./landing-demos-register.md)

After registration, the template appears in the website or page creation wizard.

You can retrieve the list of registered templates using the [landing.demos.getList](./landing-demos-get-list.md) method. Separate lists of templates can be obtained using the [landing.demos.getSiteList](./landing-demos-get-site-list.md) method for websites and the [landing.demos.getPageList](./landing-demos-get-page-list.md) method for pages.

## How to Prepare the Template

Before exporting, check the website itself:

- pages are correctly linked to each other
- the necessary blocks and themes are used
- images and external resources are accessible via working URLs
- title, description, and preview data are prepared for the template

If the website is multi-page, use a single theme for all pages. This helps maintain a consistent appearance after the template is installed.

## How to Register the Template

The minimum process is as follows:

1. Create a website or page that will serve as the basis for the template
2. Export the website using the [landing.site.fullExport](../site/landing-site-full-export.md) method
3. Save the export result on the application side
4. When installing the application, pass the exported array to [landing.demos.register](./landing-demos-register.md)
5. Verify that the template appears in the website or page creation wizard

Typically, the result of `landing.site.fullExport` is passed to `landing.demos.register` without manual restructuring of the data.

If you need to change the title, description, preview, or localization, these values should be set in the export and registration parameters.

After registration, the template can be verified using the [landing.demos.getList](./landing-demos-get-list.md) method.

## What to Consider Before Registration

**Template Type.** The template data uses the `type` and `tpl_type` fields. These define the template type and its usage location in the wizard. Check these fields in the structure you pass to [landing.demos.register](./landing-demos-register.md).

**Export Composition.** Usually, the complete result of [landing.site.fullExport](../site/landing-site-full-export.md) is passed for registration. If the structure is manually modified, ensure that the required fields and the page map `items` are preserved.

**Preview Data.** Prepare `preview`, `preview2x`, `preview3x`, and `preview_url` if the template should be displayed in the list and in the preview.

**External Template Code.** To delete the template, its external code from the `XML_ID` field is required. You can obtain it using the [landing.demos.getList](./landing-demos-get-list.md) method. The [landing.demos.unregister](./landing-demos-unregister.md) method removes records by this code within the current application. If both the website template and the page template are registered with the same code, deletion may affect both related records.

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
- [Export a Website](../site/landing-site-full-export.md)
- [Template Localization](./localization.md)