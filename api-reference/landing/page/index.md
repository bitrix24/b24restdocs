# Entity Fields Page

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- links to pages that have not yet been created are not provided (view template)

{% endnote %}

{% endif %}

#|

|| **Fields** | **Description** | **Read** | **Write** ||
|| **ID**
[`unknown`](../../data-types.md) | Page identifier. Created automatically and unique within the database. | Yes | No ||
|| **CODE^*^**
[`unknown`](../../data-types.md) | Unique symbolic code for the page. Added to the website address if it is not the main page. | Yes | Yes ||
|| **RULE**
[`unknown`](../../data-types.md) | Regular expression for displaying the page by mask. For example, the rule `section/([\d]+)` for a page at the root of the site will match all pages of the form `/section/<n>/`, where <n> is any number. | Yes | No ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Page activity: Y / N. | Yes | No ||
|| **DELETED**
[`unknown`](../../data-types.md) | Flag for [deleted page](*deleted_page): Y / N.  | Yes | Yes ||
|| **TITLE^*^**
[`unknown`](../../data-types.md) | Title of the page. | Yes | Yes ||
|| **XML_ID**
[`unknown`](../../data-types.md) | External key for developer needs. Not used by the service. | Yes | Yes ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | Arbitrary description of the page. Displayed in the list of pages. | Yes | Yes ||
|| **SITE_ID^*^**
[`unknown`](../../data-types.md) | Identifier of the site to which the page is linked. | Yes | Yes ||
|| **CREATED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who created the page. | Yes | No ||
|| **MODIFIED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who modified the page. | Yes | No ||
|| **DATE_CREATE**
[`unknown`](../../data-types.md) | Creation date. | Yes | No ||
|| **DATE_MODIFY**
[`unknown`](../../data-types.md) | Modification date. | Yes | No ||
|| **SITEMAP**
[`unknown`](../../data-types.md) | The page is present in the sitemap (`/sitemap.xml`), Y / N. | Yes | Yes ||
|| **FOLDER_ID**
[`unknown`](../../data-types.md) | Identifier of the folder where the page is located. | Yes | Yes ||
|| **TPL_ID**
[`unknown`](../../data-types.md) | Identifier of the [view template](../template/index.md). | Yes | Yes ||
|| **TPL_CODE**
[`unknown`](../../data-types.md) | Identifier of the partner solution template on which the site was created. For example, bitrix.eshop. | Yes | No ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

[*deleted_page]: Entities marked as deleted do not appear in any requests. The system does not see them. Through REST, you can access such entities only by explicitly specifying DELETED=Y in the filter.