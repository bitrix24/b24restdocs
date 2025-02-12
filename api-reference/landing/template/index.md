# View Template Object

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

The object describes the templates for the page view (with header, footer, sidebar).

> Quick navigation: [all methods](#all-methods)

View templates describe the views of a specific site or page regarding how their layout appears:

- With header and footer
- With left sidebar
- With right sidebar
- Their variations

The binding to the page is stronger than the binding to the site (if both the site and the page of that site are bound to different templates, the output will occur according to the page template grid). Each area of such a template is a separate page (with a set of blocks and other similar characteristics).

Currently, the system has several pre-installed view templates, and there is no option for third-party developers to expand them.

Here is the list:

#|
|| **External Code (XML_ID)** | **Name** ||
|| **empty** | Empty ||
|| **sidebar_left** | With left sidebar ||
|| **sidebar_right** | With right sidebar ||
|| **header_footer** | With header and footer ||
|| **without_left** | Without left sidebar ||
|| **without_right** | Without right sidebar ||
|#

Please note that template identifiers are not specified here; however, when passing to the [site](../site/base-fields.md) or [page](../page/index.md), you need to specify the identifier. To obtain the identifier directly, use the method [landing.template.getlist](./landing-template-get-list.md). You will receive a list of identifiers specific to your account.

## Overview of Methods {#all-methods}

#|
|| **Method** | **Description** ||
|| [landing.template.getLandingRef](./landing-template-get-landing-ref.md) | Retrieves a list of included areas for the page ||
|| [landing.template.getlist](./landing-template-get-list.md) | Retrieves a list of templates ||
|| [landing.template.getSiteRef](./landing-template-get-site-ref.md) | Retrieves a list of included areas for the site ||
|| [landing.template.setLandingRef](./landing-template-set-landing-ref.md) | Sets the included areas for the page ||
|| [landing.template.setSiteRef](./landing-template-set-site-ref.md) | Sets the included areas for the site ||
|#