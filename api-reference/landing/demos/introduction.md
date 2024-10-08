# Introduction to Templates

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

User templates allow you to add your own template to the site or page creation wizard. For a developer, it is easy to create their own site and distribute it among clients. Here’s how to do it.

1. First, you need to create a site and pages (or a single-page site), setting up links both within pages and between pages. You can create from scratch or based on one of the ready-made templates included with the product.

{% note warning %}

Some templates were built on an old version of the API, and their export may not work properly. We are already working on fixing the issues, but it will take time.

{% endnote %}

2. On your account, call the method [landing.site.fullExport](../site/landing-site-full-export.md), and save the result in some storage. For example, a JSON file.

3. In your application, when installing it, call [landing.demos.register](./landing-demos-register.md), passing this array. The format of the array is not described, and its modification is at the developer's discretion.

4. If you need more blocks, you can order layout services from specialists or use the additional blocks offered by the [vendor](https://htmlstream.com/preview/unify-v2.6/unify-main/shortcodes/index.html).

**Important Points:**

- If the site contains a single page, then upon installation (p.3), the template will be added to both the site creation wizard and the page creation wizard. Otherwise, only the template for the site creation wizard will be added.
- If you exported a store, the template will only appear in stores. If it’s a site, then only in sites. An exception is single-page sites, which also appear among the templates in the page creation wizard within the store.
- If a partner block is placed on the page you created, you must ensure in advance that the necessary partner blocks are already registered in the system when calling **landing.demos.register**.
- When the application is deleted, the templates and blocks of the application are also removed, except for pages that were created based on templates.

## Preview URL

This link is displayed in the template preview when the user selects it. You can obtain a link for registering the template as follows:

1. Create a site/page in the cloud Bitrix24 based on this template. The page may only minimally differ from the template you are registering, so that users creating sites and pages do not have any questions.
2. If it is a multi-page site, the main page is sufficient.
3. Publish the site and specify the obtained link in the "Preview URL" field.

{% note warning %}

Monitor the activity of your account to ensure it is not automatically deleted, which would lead to the removal of preview links. This, in turn, would deactivate your solutions.

{% endnote %}

## Images

Images are saved in the template as absolute links to your address (the one from which you exported the template). They will link to your address until the user changes them to their own images.

## Color Palette

If the site is multi-page, it is highly recommended to create all pages of the site in [one of the themes](../page/color-themes.md).