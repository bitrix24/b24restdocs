# Entity Fields Site

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- mandatory parameters are not specified
- links to pages that have not yet been created (view template) are not listed

{% endnote %}

{% endif %}

#|
|| **Field** | **Description** | **Read** | **Write** ||
|| **ID**
[`unknown`](../../data-types.md) | Site identifier. Created automatically and unique within the database. | Yes | No ||
|| **CODE^*^**
[`unknown`](../../data-types.md) | Unique symbolic code of the site. Does not appear in the cloud B24. | Yes | Yes ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Site activity: Y / N. | Yes | No ||
|| **TYPE^*^**
[`unknown`](../../data-types.md) | Type of site. (PAGE – regular site, STORE – store). Type of site. Be sure to familiarize yourself with the concept of [scopes](../types.md). | Yes | Yes ||
|| **DELETED**
[`unknown`](../../data-types.md) | Flag for [deleted page](*deleted_page): Y / N. | Yes | Yes ||
|| **TITLE^*^**
[`unknown`](../../data-types.md) | Title of the site. | Yes | Yes ||
|| **XML_ID**
[`unknown`](../../data-types.md) | External key for developer needs. Not used by the service. | Yes | Yes ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | Arbitrary description of the site. Displayed in the list of sites. | Yes | Yes ||
|| **DOMAIN_ID**
[`unknown`](../../data-types.md) | If the field is provided and it is empty, a random domain in the bitrix24.site zone will be created. Otherwise, a new specified account in the bitrix24.site zone will be registered if it is not occupied. By default, a random domain is created. | No | Yes ||
|| **DOMAIN_NAME**
[`unknown`](../../data-types.md) | Domain of the site. | Yes | Yes ||
|| **LANDING_ID_INDEX**
[`unknown`](../../data-types.md) | ID of the page designated as the main page of the site. | Yes | Yes ||
|| **LANDING_ID_404**
[`unknown`](../../data-types.md) | ID of the page designated as the site's 404 error page. | Yes | Yes ||
|| **TPL_ID**
[`unknown`](../../data-types.md) | Identifier of the [view template](.). | Yes | Yes ||
|| **LANG**
[`unknown`](../../data-types.md) | This field is relevant only for cloud Bitrix24, containing the language identifier that will be used for the site (some language phrases will change, for example, in the order form). | Yes | Yes ||
|| **CREATED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who created it. | Yes | No ||
|| **MODIFIED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who modified it. | Yes | No ||
|| **DATE_CREATE**
[`unknown`](../../data-types.md) | Creation date. | Yes | No ||
|| **DATE_MODIFY**
[`unknown`](../../data-types.md) | Modification date. | Yes | No ||
|| **PUBLIC_URL**
[`unknown`](../../data-types.md) | Public URL of the site. | Yes | No ||
|| **PREVIEW_PICTURE**
[`unknown`](../../data-types.md) | Preview image of the site (its main page). | Yes | No ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}


[*deleted_page]: Entities marked as deleted do not appear in any requests. The system does not see them. You can only access such entities through REST by explicitly specifying DELETED=Y in the filter.