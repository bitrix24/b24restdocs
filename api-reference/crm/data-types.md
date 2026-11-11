# Data Types and Object Structure in the CRM REST API

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

In addition to the basic types, fields of CRM methods in the previous API version use CRM types: identifiers of items and directory entries, multifields, files, addresses, and object type codes. The field type is specified in the `type` key, which is returned by field retrieval methods such as [crm.item.fields](./universal/crm-item-fields.md). The response format is described in the [Field Description](#crm_rest_field_description) section.

Use this reference when:

- a field has a CRM type, such as `crm_status`, `crm_multifield`, or `crm_entity`
- you need the numeric or symbolic code of a CRM object type
- you need to build a value to write: a [binding to a CRM item](#crm-binding-format), an [activity file](#diskfile) or a [timeline comment file](#attached_diskfile), or an [address](#crm_user_field_address)

{% note info "" %}

This reference describes the types of the previous API version — the `crm.deal.*`, `crm.item.*`, and other methods called at `/rest/`. The value formats of the [Bind To CRM Items](#crm-binding-format) and [Address](#crm_user_field_address) user fields are described for both versions.

- For new integrations, use [REST 3.0](../rest-v3.md) — its methods are called at `/rest/api/`. Many methods have kept their names, such as `crm.deal.get`; equivalents for the rest are listed in [{#T}](./migration.md).
- Currencies, SPA types, the timeline, and some other methods are available only in the previous version for now. See the list in the [What REST 3.0 Does Not Support Yet](./migration.md#previous-only) section.
- REST 3.0 describes fields differently: the field type is returned by [crm.&lt;type&gt;.field.list](./rest-v3/fields/crm-entity-field-list-rest-v3.md), and the field sets are listed on the object pages, for example [Contact Fields in REST 3.0](./rest-v3/items/fields-contact.md).
- Both versions use the numeric `entityTypeId` identifiers from the [CRM Object Type](#object_type) table and the [language identifiers](#lang-ids). The short `entityTypeAbbr` codes are used only in the previous version. REST 3.0 does not accept `userFieldEntityId` codes — the method name defines the object type — but returns them in the `entityId` field of the [crm.&lt;type&gt;.field.custom.*](./rest-v3/fields/index.md) methods.

{% endnote %}

Described in other articles:

- the basic types `integer`, `string`, `datetime`, `boolean`, and others — in [Data Types and Parameter Formats in REST API](../data-types.md#standart-types); compound notations such as `type[]` — in the [How Compound Types Are Written](../data-types.md#compound-types) section
- CRM form types `crm_form*` — in [{#T}](./form/data-types.md)

The `object.field` notation, such as `catalog_product.id`, means the value of the `id` field of the `catalog_product` object.

## Data Types

#|
|| **Type** | **Description** ||
|| `crm_status` | String identifier of a CRM directory entry (e.g., `NEW`). To retrieve the possible values of a specific directory, use the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID`. The directory code for the filter is returned in the `statusType` key of the [field description](#crm_rest_field_description).

In REST 3.0:

- the `DEAL_TYPE`, `CONTACT_TYPE`, `COMPANY_TYPE`, `HONORIFIC`, `EMPLOYEES`, `INDUSTRY`, and `SOURCE` directories are returned by the [crm.dictionary.&lt;directory&gt;.*](./rest-v3/dictionaries/index.md) methods. The directory name is the code without underscores in lowercase: `DEAL_TYPE` → `crm.dictionary.dealtype.list`
- deal stages `DEAL_STAGE` and `DEAL_STAGE_{pipeline id}` and SPA stages `DYNAMIC_{entityTypeId}_STAGE_{pipeline id}` are returned by the [crm.&lt;type&gt;.category.stage.list](./rest-v3/categories/crm-entity-category-stage-list-rest-v3.md) method
- lead stages `STATUS`, estimate stages `QUOTE_STATUS`, invoice stages `SMART_INVOICE_STAGE_{pipeline id}`, old invoice statuses `INVOICE_STATUS`, and call list statuses `CALL_LIST` are not available in REST 3.0 — retrieve them with the [crm.status.list](./status/crm-status-list.md) method of the previous version
||
|| `crm_lead` | Integer identifier of a CRM lead. To retrieve the lead: [crm.lead.get in REST 3.0](./rest-v3/items/crm-entity-get-rest-v3.md) or [crm.lead.get in the Previous Version](./leads/crm-lead-get.md) ||
|| `crm_deal` | Integer identifier of a CRM deal. To retrieve the deal: [crm.deal.get in REST 3.0](./rest-v3/items/crm-entity-get-rest-v3.md) or [crm.deal.get in the Previous Version](./deals/crm-deal-get.md) ||
|| `crm_contact` | Integer identifier of a CRM contact. To retrieve the contact: [crm.contact.get in REST 3.0](./rest-v3/items/crm-entity-get-rest-v3.md) or [crm.contact.get in the Previous Version](./contacts/crm-contact-get.md) ||
|| `crm_company` | Integer identifier of a CRM company. To retrieve the company: [crm.company.get in REST 3.0](./rest-v3/items/crm-entity-get-rest-v3.md) or [crm.company.get in the Previous Version](./companies/crm-company-get.md) ||
|| `crm_quote` | Integer identifier of a CRM estimate. To retrieve the estimate: [crm.quote.get in REST 3.0](./rest-v3/items/crm-entity-get-rest-v3.md) or [crm.quote.get in the Previous Version](./quote/crm-quote-get.md) ||
|| `crm_category` | Integer identifier of a CRM pipeline. To retrieve the pipeline: [crm.&lt;type&gt;.category.get in REST 3.0](./rest-v3/categories/crm-entity-category-get-rest-v3.md) or [crm.category.get in the Previous Version](./universal/category/crm-category-get.md) ||
|| `crm_entity` | Integer identifier of a CRM item. The object type is returned in the `settings.parentEntityTypeId` key of the [field description](#crm_rest_field_description) and is repeated in the field name. For example, the `parentId2` field stores a deal identifier: `2` is the `entityTypeId` of deals. To retrieve the item:

- in REST 3.0, call [crm.&lt;type&gt;.get](./rest-v3/items/crm-entity-get-rest-v3.md) and pass `id` from the field value. Take the `<type>` segment from the table on the method page: `crm.deal.get` for `entityTypeId = 2`, `crm.smartprocess1038.get` for an SPA with `entityTypeId = 1038`
- in the previous version, call [crm.item.get](./universal/crm-item-get.md) and pass `entityTypeId` and `id`
||
|| `lang_map` | Object in the following format:

```text
{
    lang_1: value_1,
    lang_2: value_2,
    ..
    lang_n: value_n,
}
```

where:

- `lang_n` — [language identifier](#lang-ids)
- `value_n` — value for the `lang_n` language
||
|| `crm_dynamic_type.id` | Integer identifier of an SPA — the `id` field of the [type](#type) object. ||
|| `crm_userfield` | Value of a user field. This is how the type is written in the field tables on the `crm.item.*` method pages. The specific value type depends on the type of the field itself — see [{#T}](./universal/user-defined-fields/crm-userfield-types.md). In field descriptions, a user field is returned with its own type, such as `enumeration`, `crm`, `address`, `money`, or `employee`. The value formats of the `crm` and `address` types are described in the [Bind To CRM Items](#crm-binding-format) and [Address](#crm_user_field_address) sections ||
|| `userFieldEntityId` | String code of the CRM object type to which the user field is bound. For example, `CRM_CONTACT`. The codes of system types and the rule for SPAs are in the [CRM Object Type](#object_type) section ||
|| `location` | String identifier of a location from the Bitrix24 location directory. A service type used to calculate taxes by location: fields of this type, such as `LOCATION_ID` of a deal, are filled in only when this tax calculation is enabled in Bitrix24 ||
|| [`crm_item_product_row`](#crm_item_product_row) | Product item object of a CRM item. The structure in the previous version is in the [crm_item_product_row](#crm_item_product_row) section, and in REST 3.0 — in the [Product Item Fields](./rest-v3/product-rows/index.md#fields) section. The item identifier is written as `crm_item_product_row.id`. To retrieve product items: in REST 3.0, use [crm.&lt;type&gt;.productrow.list](./rest-v3/product-rows/crm-entity-product-row-list-rest-v3.md); in the previous version, use [crm.item.productrow.list](./universal/product-rows/crm-item-productrow-list.md) ||
|| [`crm_multifield`](#crm_multifield) | Object describing a "multifield". Multifields store phone numbers, email addresses, and other contact information in leads, contacts, companies, and SPAs with `isMultifieldsEnabled` turned on ||
|| [`crm_currency`](#crm_currency) | CRM currency. In object fields, such as `CURRENCY_ID` of a deal, this is a string currency code: `EUR`, `USD`. The full currency object is returned by the [crm.currency.get](./currency/crm-currency-get.md) method ||
|| [`crm_currency_localization`](#crm_currency_localization) | Object describing currency localization. In the [crm.currency.fields](./currency/crm-currency-fields.md) response, the field with these objects has the `currency_localization` type ||
|| [`crm_orderentity`](#crm_orderentity) | Object describing the link between a CRM item and an online store order. Returned by the [crm.orderentity.*](./universal/order-entity/index.md) methods ||
|| `recurring_params` | Object with the parameters for calculating the next repetition date for a recurring CRM object template. The field set depends on the object and is described on the page of the specific method. For example, [fields of the PARAMS object](./deals/recurring-deals/crm-deal-recurring-fields.md#params-fields) for recurring deals ||
|| [`type`](#type) | Object describing a custom CRM object type (SPA) ||
|| [`type.relations`](#typerelations) | Object with the links between an SPA and other CRM object types ||
|| [`relation`](#relation) | Object describing a single link between an SPA and a CRM object type ||
|| [`type.linkedUserFields`](#typelinkeduserfields) | Object with user fields of other modules in which SPA items can be selected ||
|| [`diskfile`](#diskfile) | File retained on Bitrix24 Drive ||
|| [`attached_diskfile`](#attached_diskfile) | File attached to a timeline comment ||
|| `product_file` | Product image in the old product card — the `PREVIEW_PICTURE` and `DETAIL_PICTURE` fields in the deprecated `crm.product.*` methods. The response contains an object with the keys `id` — the file identifier, `showUrl` — the relative address for viewing, and `downloadUrl` — the relative address for downloading. If there is no image — `null` ||
|| `crm_webform` | Integer identifier of the CRM form through which the item was created. The form data is returned by the [crm.form.get](./form/crm-form-get.md) method ||
|| `crm_repeat_sale_segment_title` | Integer identifier of the repeat sale scenario that created the deal. Despite the type name, the field contains the identifier, not the scenario name. The `repeatSaleSegmentId` field of this type is multiple and read-only. It is returned in [crm.item.fields](./universal/crm-item-fields.md) only if repeat sales are available in Bitrix24 ||
|| [`crm_activity_binding`](#crm_activity_binding) | Object describing the binding of an activity to a CRM item ||
|| `crm_activity_communication` | Object describing the communication channel of an activity. The field set is in [{#T}](./timeline/activities/activity-base/crm-activity-communication-fields.md). In REST 3.0, communication channels are returned in the `communications` field; its format is in the [Activity Fields](./rest-v3/activities/index.md#fields) section ||
|#

## Activity Enumerations {#activity-enums}

Activity fields in the [crm.activity.*](./timeline/activities/activity-base/index.md) methods of the previous version reference service enumerations. The field value is an integer from a fixed set. In REST 3.0, activity fields work differently: the direction and the description format are passed as strings, such as `incoming` or `html`, and bindings as `entityTypeId` and `entityId` pairs. See the format in the [Activity Fields](./rest-v3/activities/index.md#fields) section.

#|
|| **Type** | **Values** ||
|| `crm_enum_activitytype` | Type of the activity: `1` — meeting, `2` — call, `3` — task, `4` — email, `5` — general type for importing calendar events, `6` — general type for activities managed by providers. `0` — type is not defined ||
|| `crm_enum_activitystatus` | Status of the activity: `1` — pending, `2` — completed, `3` — completed automatically. `0` — status is not defined ||
|| `crm_enum_activitypriority` | Importance of the activity: `1` — low, `2` — medium, `3` — high. `0` — not specified ||
|| `crm_enum_activitynotifytype` | Unit of time for the reminder: `1` — minutes, `2` — hours, `3` — days. `0` — reminder is not specified ||
|| `crm_enum_activitydirection` | Direction of the activity: `1` — incoming, `2` — outgoing. `0` — direction is not defined. Applies to calls and emails ||
|| `crm_enum_contenttype` | Format of the description text: `1` — plain text, `2` — BB codes, `3` — HTML. `0` — format is not defined ||
|| `crm_enum_ownertype` | Integer identifier of the CRM object type to which the record belongs — the `entityTypeId` value from the [CRM Object Type](#object_type) table ||
|#

## Object Structure

### crm_multifield {#crm_multifield}

The `crm.lead.*`, `crm.contact.*`, and `crm.company.*` methods return the keys in uppercase, as in the table. In the `crm.item.*` methods, the same data is returned in the `fm` field — see [CRM Object Fields](./universal/object-fields.md#fm) for details.

To write in the previous version, pass an array of objects to the field, for example `PHONE`:

- add a value — `VALUE` and `VALUE_TYPE`
- change a value — `ID`, the new `VALUE`, and `VALUE_TYPE`. Without `VALUE_TYPE`, the type is reset to `WORK`, or to `OTHER` for `IM`
- delete a value — `ID` and `DELETE: "Y"` or an empty `VALUE`

See examples on the [crm.contact.update](./contacts/crm-contact-update.md) page.

In REST 3.0, phone numbers, email, websites, and messengers are separate fields `phone`, `email`, `web`, and `im`; their format is described in the [Phone Numbers, Email, Websites, and Messengers](./rest-v3/items/fields-contact.md#multifield) section.

#|
|| **Value**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md#standart-types) | Identifier of the multifield value ||
|| **TYPE_ID**
[`string`](../data-types.md#standart-types) | Type of the multifield: `PHONE`, `EMAIL`, `WEB`, `IM`, or the service type `LINK` ||
|| **VALUE**
[`string`](../data-types.md#standart-types) | String value of the multifield ||
|| **VALUE_TYPE**
[`string`](../data-types.md#standart-types) | Type of the multifield value. The possible values depend on `TYPE_ID`:
- `PHONE` — `WORK`, `MOBILE`, `FAX`, `HOME`, `PAGER`, `MAILING`, `OTHER`
- `EMAIL` — `WORK`, `HOME`, `MAILING`, `OTHER`
- `WEB` — `WORK`, `HOME`, `FACEBOOK`, `VK`, `LIVEJOURNAL`, `TWITTER`, `OTHER`
- `IM` — `TELEGRAM`, `VK`, `FACEBOOK`, `INSTAGRAM`, `VIBER`, `BITRIX24`, `OPENLINE`, `IMOL`, `OTHER`. The deprecated `SKYPE`, `ICQ`, `MSN`, and `JABBER` are available only if they are already used in Bitrix24
- `LINK` — `USER`
||
|#

### crm_item_product_row {#crm_item_product_row}

Product item of a CRM item in the [crm.item.productrow.*](./universal/product-rows/index.md) methods of the previous version. In REST 3.0, the item fields are different, for example `ownerTypeId` instead of `ownerType` — see [Product Item Fields](./rest-v3/product-rows/index.md#fields).

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md#standart-types) | Identifier of the product item ||
|| **ownerId**
[`integer`](../data-types.md#standart-types) | Identifier of the CRM object ||
|| **ownerType**
[`string`](../data-types.md#standart-types) | Short symbolic code of the CRM object type — a value from the `entityTypeAbbr` column of the [CRM Object Type](#object_type) table. For example, `D` for a deal ||
|| **productId**
[`catalog_product.id`](../catalog/data-types.md#catalog_product) | Identifier of the product from the catalog ||
|| **productName**
[`string`](../data-types.md#standart-types) | Name of the product in the product item ||
|| **price**
[`double`](../data-types.md#standart-types) | Price per unit of the product item, including discounts and taxes ||
|| **priceAccount**
[`double`](../data-types.md#standart-types) | Price per unit of the product item, including discounts and taxes, converted to the reporting currency ||
|| **priceExclusive**
[`double`](../data-types.md#standart-types) | Price per unit of the product item, including discounts but excluding taxes ||
|| **priceNetto**
[`double`](../data-types.md#standart-types) | Price per unit of the product item, excluding discounts and taxes ||
|| **priceBrutto**
[`double`](../data-types.md#standart-types) | Price per unit of the product item, including taxes but excluding discounts ||
|| **quantity**
[`double`](../data-types.md#standart-types) | Quantity of the product ||
|| **discountTypeId**
[`integer`](../data-types.md#standart-types) | Type of discount.
Possible values:
- `1` — absolute value
- `2` — percentage value ||
|| **discountRate**
[`double`](../data-types.md#standart-types) | Discount value in percent ||
|| **discountSum**
[`double`](../data-types.md#standart-types) | Absolute discount value ||
|| **taxRate**
[`double`](../data-types.md#standart-types) | Tax rate in percent. Returns `null` if no tax is set for the item ||
|| **taxName**
[`string`](../data-types.md#standart-types) | Name of the tax rate. For example, `VAT 20` or `No VAT` ||
|| **taxIncluded**
[`string`](../data-types.md#standart-types) | Indicator of whether tax is included in the price.
Possible values:
- `Y` — tax included
- `N` — tax not included ||
|| **customized**
[`string`](../data-types.md#standart-types) | Deprecated field. Always `Y` in the response, ignored when writing ||
|| **measureCode**
[`catalog_measure.code`](../catalog/data-types.md#catalog_measure) | Unit of measure code ||
|| **measureName**
[`string`](../data-types.md#standart-types) | Text representation of the unit of measure. For example, pcs, kg, m, l ||
|| **sort**
[`integer`](../data-types.md#standart-types) | Sorting ||
|| **xmlId**
[`string`](../data-types.md#standart-types) | External identifier of the product item ||
|| **type**
[`integer`](../data-types.md#standart-types) | Type of product.
Possible values:
- `1` — simple product
- `2` — bundle
- `3` — product with variants
- `4` — variant
- `5` — variant without a product: the product is not specified or deleted
- `6` — invalid product with variants
- `7` — service ||
|| **storeId**
[`integer`](../data-types.md#standart-types) | Identifier of the warehouse. Filled in only if the product item has a reservation; otherwise, `null` is returned.
The field is returned by the [crm.item.productrow.get](./universal/product-rows/crm-item-productrow-get.md) and [crm.item.productrow.list](./universal/product-rows/crm-item-productrow-list.md) methods ||
|#

### crm_currency {#crm_currency}

Currency in the response of the [crm.currency.get](./currency/crm-currency-get.md) and [crm.currency.list](./currency/crm-currency-list.md) methods.

#|
|| **Value**
`type` | **Description** ||
|| **AMOUNT**
[`double`](../data-types.md#standart-types) | Exchange rate relative to the base currency.
For the base currency, it is always `1`. Precision — four decimal places
 ||
|| **AMOUNT_CNT**
[`integer`](../data-types.md#standart-types) | Nominal value.
For the base currency, it is always `1`
 ||
|| **BASE**
[`string`](../data-types.md#standart-types) | Indicator of whether the currency is the base one (`Y`/`N`) ||
|| **CURRENCY**
[`string`](../data-types.md#standart-types) | Currency identifier. Corresponds to the ISO 4217 standard ||
|| **DATE_UPDATE**
[`datetime`](../data-types.md#standart-types) | Date of the last change ||
|| **SORT**
[`integer`](../data-types.md#standart-types) | Sorting ||
|| **LID**
[`string`](../data-types.md#standart-types) | Language code for which the [localization parameters](#crm_currency_localization) are returned ||
|| **DECIMALS**
[`integer`](../data-types.md#standart-types) | Number of decimal places in the fractional part ||
|| **DEC_POINT**
[`string`](../data-types.md#standart-types) | Decimal point for output ||
|| **FORMAT_STRING**
[`string`](../data-types.md#standart-types) | Format template ||
|| **FULL_NAME**
[`string`](../data-types.md#standart-types) | Name of the currency ||
|| **THOUSANDS_SEP**
[`string`](../data-types.md#standart-types) | Thousands separator ||
|| **LANG**
[`lang_map`](#data-types) | Currency localizations. The value for each language is a [crm_currency_localization](#crm_currency_localization) object ||
|#

### crm_currency_localization {#crm_currency_localization}

Currency output parameters for one language. You can manage them with the [crm.currency.localizations.*](./currency/localizations/index.md) methods.

#|
|| **Value**
`type` | **Description** ||
|| **DECIMALS**
[`integer`](../data-types.md#standart-types) | Number of decimal places in the fractional part.

Default value — `2` ||
|| **DEC_POINT**
[`string`](../data-types.md#standart-types) | Decimal point for output.

Default value — `.` (dot) ||
|| **FORMAT_STRING**
[`string`](../data-types.md#standart-types) | Format template. Must contain the `#` symbol — the price value is substituted in its place. The field is required when adding a localization.

Examples of templates for the amount 1000:
- `$ #` — $ 1000
- `# EUR` — 1000 EUR
- `€ #` — € 1000
  ||
|| **FULL_NAME**
[`string`](../data-types.md#standart-types) | Name of the currency.

If the name is not set, the `FULL_NAME` field of the [crm_currency](#crm_currency) object returns the currency code ||
|| **HIDE_ZERO**
[`string`](../data-types.md#standart-types) | Indicator of whether to hide insignificant zeros (`Y`/`N`).

Default value — `N` ||
|| **THOUSANDS_SEP**
[`string`](../data-types.md#standart-types) | Thousands separator.

Default value — ` ` (space) ||
|| **THOUSANDS_VARIANT**
[`string`](../data-types.md#standart-types) | Code of the thousands separator.

Default value — `S`: Bitrix24 sets it when a localization is added without `THOUSANDS_VARIANT`.

If you pass a non-empty `THOUSANDS_VARIANT` value, Bitrix24 ignores `THOUSANDS_SEP` and sets the separator by code:
- `N` — empty, no thousands separator. Example: 12345678
- `C` — comma. Example: 12,345,678
- `D` — dot. Example: 12.345.678
- `S` — space. Example: 12 345 678
- `B` — non-breaking space `&nbsp;`. Example: 12 345 678. Unlike the previous option, the result is not split across lines when wrapping

If you do not need a thousands separator, pass the `THOUSANDS_VARIANT` field with the value `N`. An empty string in the `THOUSANDS_SEP` field is not allowed.

Pass a custom separator in `THOUSANDS_SEP` only as an HTML character code, for example `&#8239;` for a narrow non-breaking space. When updating a localization, do not pass `THOUSANDS_VARIANT`. When adding one, pass `THOUSANDS_VARIANT` as an empty string; otherwise, Bitrix24 sets `S` and replaces the separator with a space ||
|#

### crm_orderentity {#crm_orderentity}

Link between a CRM item and an order in the [crm.orderentity.*](./universal/order-entity/index.md) methods.

#|
|| **Value**
`type` | **Description** ||
|| **ownerId**
[`integer`](../data-types.md#standart-types) | Identifier of the CRM object ||
|| **ownerTypeId**
[`integer`](../data-types.md#standart-types) | Numeric identifier of the CRM object type — a value from the `entityTypeId` column of the [CRM Object Type](#object_type) table. For example, `2` for a deal ||
|| **orderId**
[`sale_order.id`](../sale/data-types.md#sale_order) | Identifier of the order ||
|#

### type {#type}

SPA in the [crm.type.*](./universal/user-defined-object-types/index.md) methods. The `is*` flags are returned as `Y` or `N`.

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md#standart-types)  | Identifier of the SPA ||
|| **title**
[`string`](../data-types.md#standart-types) | Title of the SPA ||
|| **code**
[`string`](../data-types.md#standart-types) | Symbolic code ||
|| **createdBy**
[`integer`](../data-types.md#standart-types)  | Identifier of the user who created the SPA. Read-only ||
|| **entityTypeId**
[`integer`](../data-types.md#standart-types)  | Numeric identifier of the CRM object type, `entityTypeId`. See the [CRM Object Type](#object_type) section for details ||
|| **isCategoriesEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether custom pipelines and sales tunnels are enabled ||
|| **isStagesEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether custom stages and the Kanban board are enabled ||
|| **isBeginCloseDatesEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether the **Start Date** and **End Date** fields are enabled ||
|| **isClientEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether the **Client** field is enabled ||
|| **isUseInUserfieldEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether the SPA can be used in user fields ||
|| **isLinkWithProductsEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether linking catalog products is enabled ||
|| **isMycompanyEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether the **Your Company Details** field is enabled ||
|| **isDocumentsEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether document printing is enabled ||
|| **isSourceEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether the **Source** and **Source Information** fields are enabled ||
|| **isObserversEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether the **Observers** field is enabled ||
|| **isRecyclebinEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether the recycle bin is enabled ||
|| **isAutomationEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether automation rules and triggers are enabled ||
|| **isBizProcEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether the workflow designer is enabled ||
|| **isSetOpenPermissions**
[`boolean`](../data-types.md#standart-types)  | Whether new pipelines are made available to everyone ||
|| **isRecurringEnabled**
[`boolean`](../data-types.md#standart-types) | Whether the **Recurrence** field is enabled ||
|| **isMultifieldsEnabled**
[`boolean`](../data-types.md#standart-types) | Whether communication fields are enabled: phone, email, and others ||
|| **isStartCatalogEnabled**
[`boolean`](../data-types.md#standart-types) | Whether the process launch catalog is enabled ||
|| **isCreateByFormEnabled**
[`boolean`](../data-types.md#standart-types) | Whether creating items through a CRM form is enabled ||
|| **startCatalogFormId**
[`integer`](../data-types.md#standart-types) | Identifier of the CRM form for creating items. If no form is set — `null` ||
|| **daysBeforeClose**
[`integer`](../data-types.md#standart-types) | Number of days from creation allotted to an item before completion. If no deadline is set — `null` ||
|| **isPaymentsEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether payments are enabled. Read-only ||
|| **isCountersEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether counters are enabled. Read-only ||
|| **isInitialized**
[`boolean`](../data-types.md#standart-types) | Whether the SPA creation is complete. The value `N` occurs only during the first seconds after creation and during deletion. Read-only ||
|| **createdTime**
[`datetime`](../data-types.md#standart-types) | Time the SPA was created. Read-only ||
|| **updatedTime**
[`datetime`](../data-types.md#standart-types) | Time the SPA was modified. Read-only ||
|| **updatedBy**
[`integer`](../data-types.md#standart-types)  | Identifier of the user who modified the SPA. Read-only ||
|| **relations**
[`type.relations`](#typerelations) | Links between the SPA and other CRM object types ||
|| **linkedUserFields**
[`type.linkedUserFields`](#typelinkeduserfields) | User fields of other modules in which SPA items can be selected ||
|| **customSections**
[`array`](../data-types.md#standart-types)    | List of all digital workplaces

The field is deprecated. To work with digital workplaces, use the [`crm.automatedsolution.*`](./automated-solution/index.md) methods ||
|| **additionalCustomSectionIds**
[`integer[]`](../data-types.md#standart-types) | Identifiers of additional digital workplaces in which the SPA is displayed ||
|| **customSectionId**
[`integer`](../data-types.md#standart-types)  | Identifier of the digital workplace

The field is deprecated. To work with digital workplaces, use the [`crm.automatedsolution.*`](./automated-solution/index.md) methods ||
|#

### type.relations {#typerelations}

Links of an SPA — the `relations` field of the [type](#type) object.

#|
|| **Value**
`type` | **Description** ||
|| **parent**
[`relation[]`](#relation) | CRM object types that can be parents of the SPA ||
|| **child**
[`relation[]`](#relation) | CRM object types for which the SPA can be a parent ||
|#

#### relation {#relation}

A single link from the `parent` and `child` arrays. The `is*` flags are returned as `Y` or `N`.

#|
|| **Value**
`type` | **Description** ||
|| **entityTypeId**
[`integer`](../data-types.md#standart-types)  | Numeric identifier of the CRM object type `entityTypeId`: a system type or an SPA. See the values in the [CRM Object Type](#object_type) section ||
|| **isChildrenListEnabled**
[`boolean`](../data-types.md#standart-types)  | Whether to show the list of child items in the parent card. When writing in [crm.type.add](./universal/user-defined-object-types/crm-type-add.md) and [crm.type.update](./universal/user-defined-object-types/crm-type-update.md), pass the string `"true"` or `"false"`: the values `Y` and `N` turn the list off ||
|| **isPredefined**
[`boolean`](../data-types.md#standart-types)  | Whether the link is predefined (system) ||
|#

### type.linkedUserFields {#typelinkeduserfields}

User fields of other modules in which SPA items can be selected — the `linkedUserFields` field of the [type](#type) object. Bitrix24 retains it only if `isUseInUserfieldEnabled` is turned on for the SPA.

- The key is a string in the `{field object}|{field code}` format, for example `TASKS_TASK|UF_CRM_TASK`. This is a single key; the `|` character does not split it.
- The value in the response is `Y` if SPA items can be selected in this field, and `N` if they cannot.

#|
|| **Value**
`type` | **Description** ||
|| **CALENDAR_EVENT\|UF_CRM_CAL_EVENT**
[`boolean`](../data-types.md#standart-types) | Calendar event ||
|| **TASKS_TASK\|UF_CRM_TASK**
[`boolean`](../data-types.md#standart-types) | Tasks ||
|| **TASKS_TASK_TEMPLATE\|UF_CRM_TASK**
[`boolean`](../data-types.md#standart-types) | Task templates ||
|#

{% note warning "" %}

The `boolean` type in the table describes the response: `Y` or `N`. When writing in [crm.type.add](./universal/user-defined-object-types/crm-type-add.md) and [crm.type.update](./universal/user-defined-object-types/crm-type-update.md), pass only the string `"true"` — any other value, including `Y`, turns the field off.

{% endnote %}

### diskfile {#diskfile}

Activity file retained on Bitrix24 Drive. This type is used by the `FILES` field and the deprecated `WEBDAV_ELEMENTS` field in the [crm.activity.*](./timeline/activities/activity-base/index.md) methods. The field is multiple: the response contains an array of objects. In REST 3.0, activity files are returned in the `files` field with the keys `id`, `name`, `url`, and `downloadUrl`; see the [Activity Fields](./rest-v3/activities/index.md#fields) section for details.

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md#standart-types) | Identifier of the file ||
|| **url**
[`string`](../data-types.md#standart-types) | Address for downloading the file ||
|#

To write a value to a field of this type, pass the file upload structure according to the [rules for working with files](../files/how-to-upload-files.md).

### attached_diskfile {#attached_diskfile}

File attached to a timeline comment — the `FILES` field in the [crm.timeline.comment.*](./timeline/comments/index.md) methods.

In the response, the field is an object whose keys are file identifiers on Drive and whose values are file descriptions:

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md#standart-types) | Identifier of the file on Drive ||
|| **date**
[`datetime`](../data-types.md#standart-types) | Date the file was uploaded ||
|| **type**
[`string`](../data-types.md#standart-types) | Kind of file: `file`, `image`, or `video` ||
|| **name**
[`string`](../data-types.md#standart-types) | Name of the file ||
|| **size**
[`integer`](../data-types.md#standart-types) | File size in bytes ||
|| **image**
[`object`](../data-types.md#standart-types) \| [`boolean`](../data-types.md#standart-types) | Image dimensions `{"width": ..., "height": ...}` for images and videos. For other files — `false` ||
|| **authorId**
[`integer`](../data-types.md#standart-types) | Identifier of the user who uploaded the file ||
|| **authorName**
[`string`](../data-types.md#standart-types) | Name of the user who uploaded the file ||
|| **urlPreview**
[`string`](../data-types.md#standart-types) | Preview address. For files without a preview — `null` ||
|| **urlShow**
[`string`](../data-types.md#standart-types) | Address for viewing an image. For other files, it matches `urlDownload` ||
|| **urlDownload**
[`string`](../data-types.md#standart-types) | Address for downloading the file ||
|#

Example field value:

```json
{
    "11051": {
        "id": 11051,
        "date": "2026-11-10T15:20:58+01:00",
        "type": "file",
        "name": "check.txt",
        "size": 5,
        "image": false,
        "authorId": 1,
        "authorName": "Klaus Weber",
        "urlPreview": null,
        "urlShow": "https://example.bitrix24.com/disk/downloadFile/11051/?&ncc=1&filename=check.txt",
        "urlDownload": "https://example.bitrix24.com/disk/downloadFile/11051/?&ncc=1&filename=check.txt"
    }
}
```

The value is written the same way as for [diskfile](#diskfile).

### crm_activity_binding {#crm_activity_binding}

Binding of an activity to a CRM item in the `crm.activity.*` methods of the previous version. The field is multiple and read-only. In REST 3.0, bindings are returned in the `bindings` field as `entityTypeId` and `entityId` pairs; see the [Activity Fields](./rest-v3/activities/index.md#fields) section for details.

#|
|| **Value**
`type` | **Description** ||
|| **OWNER_TYPE_ID**
[`crm_enum_ownertype`](#activity-enums) | Type of the CRM object to which the activity is bound ||
|| **OWNER_ID**
[`integer`](../data-types.md#standart-types) | Identifier of the CRM item to which the activity is bound ||
|#

## Field Description {#crm_rest_field_description}

The `crm_rest_field_description` structure describes a single field in the response of field retrieval methods, such as [crm.item.fields](./universal/crm-item-fields.md) or [crm.deal.fields](./deals/crm-deal-fields.md). The response is an object where the key is the field code and the value is a description in this format. The `is*` flags are returned as JSON values `true` or `false`. In REST 3.0, the field description is structured differently; see its format on the [crm.&lt;type&gt;.field.list](./rest-v3/fields/crm-entity-field-list-rest-v3.md#item) page.

#|
|| **Value**
`type` | **Description** ||
|| **type**
[`string`](../data-types.md#standart-types) | Field type: a basic type, a CRM type from this reference, or a user field type ||
|| **isRequired**
[`boolean`](../data-types.md#standart-types) | Whether the field is required ||
|| **isReadOnly**
[`boolean`](../data-types.md#standart-types) | Whether the field is read-only. The value of such a field cannot be passed when creating or updating an item ||
|| **isImmutable**
[`boolean`](../data-types.md#standart-types) | Whether the field can be filled in only when creating an item. The value cannot be changed later ||
|| **isMultiple**
[`boolean`](../data-types.md#standart-types) | Whether the field is multiple. Values of a multiple field are passed as an array ||
|| **isDynamic**
[`boolean`](../data-types.md#standart-types) | Whether the field is a user field ||
|| **isDeprecated**
[`boolean`](../data-types.md#standart-types) | Deprecated field flag. Returned only for deprecated fields, with the value `true` ||
|| **title**
[`string`](../data-types.md#standart-types) | Name of the field. In the methods of individual objects, such as `crm.deal.fields`, user fields have the field code here, and the name is in `formLabel` ||
|| **upperName**
[`string`](../data-types.md#standart-types) | Field code in uppercase. Returned by the universal methods `crm.item.fields`, `crm.type.fields`, and `crm.category.fields`. The methods of individual objects, such as `crm.deal.fields` and `crm.lead.fields`, do not return this key ||
|| **statusType**
[`string`](../data-types.md#standart-types) | Code of the directory that provides the field values. Returned for fields of the `crm_status` type ||
|| **items**
[`array`](../data-types.md#standart-types) | Value options. Returned for user fields of the `enumeration` type ||
|| **settings**
[`object`](../data-types.md#standart-types) | Field settings. The set depends on the field type ||
|| **listLabel**
[`string`](../data-types.md#standart-types) | Name of the user field in the list ||
|| **formLabel**
[`string`](../data-types.md#standart-types) | Name of the user field in the card ||
|| **filterLabel**
[`string`](../data-types.md#standart-types) | Name of the user field in the filter ||
|#

## CRM Object Type {#object_type}

Different methods specify the CRM object type in one of four formats:

- `entityTypeId` — for example, in the `crm.item.*` methods, in activity bindings, and in SPA links
- `entityTypeName` — for example, in the `ENTITY_TYPE` field of the [crm.timeline.comment.*](./timeline/comments/index.md) methods. The response returns the value in lowercase; case does not matter when writing
- `entityTypeAbbr` — in the values of fields bound to multiple types and in `ownerType` of product items
- `userFieldEntityId` — in user field methods

#|
|| **Object Type** | **Numeric Identifier of the Type**
`entityTypeId` | **Symbolic Code of the Type**
`entityTypeName` | **Short Symbolic Code of the Type**
`entityTypeAbbr` | **User Field Object Type**
`userFieldEntityId` ||
|| Lead | 1 | LEAD | L | CRM_LEAD ||
|| Deal | 2 | DEAL | D | CRM_DEAL ||
|| Contact | 3 | CONTACT | C | CRM_CONTACT ||
|| Company | 4 | COMPANY | CO | CRM_COMPANY ||
|| Invoice (old) | 5 | INVOICE | I | CRM_INVOICE ||
|| Invoice (new) | 31 | SMART_INVOICE | SI | CRM_SMART_INVOICE ||
|| Estimate | 7 | QUOTE | Q | CRM_QUOTE ||
|| Requisite | 8 | REQUISITE | RQ | CRM_REQUISITE ||
|| Order | 14 | ORDER | O | ORDER ||
|| CRM document | 36 | SMART_DOCUMENT | DO | CRM_SMART_DOCUMENT ||
|| B2E document | 39 | SMART_B2E_DOC | SBD | CRM_SMART_B2E_DOC ||
|| SPA | `{entityTypeId}`, for example `128` | `DYNAMIC_{entityTypeId}`, for example `DYNAMIC_128` | `T` + `entityTypeId` in lowercase hexadecimal, for example `T80` or `Tb1` | `CRM_` + `id` of the [type](#type) object, not `entityTypeId`, for example `CRM_1` ||
|#

For an SPA, `id` and `entityTypeId` are different values; see the [Smart Process Identifiers](./universal/user-defined-object-types/index.md#id) section for details. The list of object types including SPAs is returned by the [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md) method. In REST 3.0, system object types are returned by the [crm.dictionary.entitytype.list](./rest-v3/dictionaries/crm-dictionary-list-rest-v3.md) method; its response does not include SPAs — they are returned by [crm.type.list](./universal/user-defined-object-types/crm-type-list.md).

{% note warning "" %}

For new integrations, use "Invoice (new)" with `entityTypeId = 31` (`SMART_INVOICE`). The `INVOICE` type with `entityTypeId = 5` is retained for compatibility with the old invoice functionality and is not recommended for new projects.

{% endnote %}

## Bind To CRM Items User Field Value {#crm-binding-format}

The value format of a `crm` field depends on the API version and on the field settings. The settings are returned in the `settings` key of the field description: types with the value `Y` are allowed.

The field supports binding to leads, deals, contacts, companies, orders, estimates, new invoices, and SPAs. Old invoices `I` cannot be selected in it.

### In REST 3.0 {#crm-binding-format-v3}

The field is passed by its `UF_CRM_…` name. The value depends on how many types the binding is allowed to:

- one type — the numeric identifier of the item, for example `107`
- several types — an object with the `entityTypeId` and `entityId` keys, for example `{"entityTypeId": 4, "entityId": 31}`

A multiple field accepts and returns an array of such values: `[107, 31]` or `[{"entityTypeId": 3, "entityId": 2571}, {"entityTypeId": 4, "entityId": 31}]`. REST 3.0 does not accept prefixed strings such as `C_2571`.

### In the Previous Version {#crm-binding-format-previous}

The value format of a `crm` field depends on how many object types the binding is allowed to:

- The binding is allowed to one type — the value contains only the item identifier without a prefix. The response returns it as a string, for example `"107"`.
- The binding is allowed to several types — the value has the `{PREFIX}_{ID}` format, where `PREFIX` is the short symbolic code of the type `entityTypeAbbr` from the [CRM Object Type](#object_type) table, and `ID` is the numeric identifier of the item. For an SPA with `entityTypeId = 177` → `b1` → `PREFIX` `Tb1`.

Examples of prefixed values:

#|
|| **Object Type** | **Example Value** ||
|| Lead | `L_4` ||
|| Deal | `D_1` ||
|| Contact | `C_2` ||
|| Company | `CO_3` ||
|| Invoice (new) | `SI_7` ||
|| SPA | `T80_6` ||
|#

If the field is multiple (`isMultiple: true`), the values are passed as an array of strings in the same format:

```json
["C_123", "CO_456"]
```

or, if one type is allowed:

```json
["107", "31"]
```

## Address User Field Value {#crm_user_field_address}

### In REST 3.0 {#crm-address-format-v3}

The value is passed and returned as an object with the `address`, `latitude`, and `longitude` keys:

```json
{"address": "Tiergartenstraße 17, 10785 Berlin, Germany", "latitude": 52.51, "longitude": 13.37}
```

### In the Previous Version {#crm-address-format-previous}

A user field of the `address` type stores the address as a single string of three parts separated by the `|` character:

```text
{address}|{latitude};{longitude}|{address ID}
```

- `{address}` — the address as text
- `{latitude};{longitude}` — coordinates separated by a semicolon. If there are no coordinates, `;` remains in their place
- `{address ID}` — identifier of the address in the Bitrix24 location module. May be missing if the value was written with coordinates

Example of a value in the response:

```text
Tiergartenstraße 17, 10785 Berlin, Germany|52.5096;13.3700|453
```

When writing, pass a string in the `{address}|{latitude};{longitude}` format — it is retained as is, without an identifier. If you pass only the address text, Bitrix24 creates the address in the location module and adds empty coordinates and the identifier itself: `{address}|;|{address ID}`.

Requisite addresses work differently: they are stored in separate fields — street, city, postal code, and others. They are described in [CRM Company Details Addresses](./requisites/addresses/index.md).

## Language Identifiers for Bitrix24 {#lang-ids}

A language identifier is a string of two Latin letters. The set of languages depends on Bitrix24: localization keys contain the installed languages. The table lists the most common languages.

#|
|| **Language Identifier** | **Language** ||
|| `ar` | Arabic ||
|| `br` | Portuguese (Brazil) ||
|| `de` | German ||
|| `en` | English ||
|| `fr` | French ||
|| `hi` | Hindi ||
|| `id` | Indonesian ||
|| `it` | Italian ||
|| `ja` | Japanese ||
|| `kz` | Kazakh ||
|| `la` | Spanish ||
|| `ms` | Malay ||
|| `pl` | Polish ||
|| `ru` | Russian ||
|| `sc` | Chinese ||
|| `tc` | Chinese (Taiwan) ||
|| `th` | Thai ||
|| `tr` | Turkish ||
|| `ua` | Ukrainian ||
|| `uz` | Uzbek ||
|| `vn` | Vietnamese ||
|#

## Continue Learning

- [{#T}](../rest-v3.md)
- [{#T}](./rest-v3/fields/crm-entity-field-list-rest-v3.md)
- [{#T}](../data-types.md#standart-types)
- [{#T}](./migration.md)
- [{#T}](./universal/object-fields.md)
- [{#T}](./universal/crm-item-fields.md)
- [{#T}](./universal/user-defined-fields/crm-userfield-types.md)
- [{#T}](./auxiliary/enum/crm-enum-owner-type.md)
