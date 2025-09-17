# Data Types and Object Structure in the REST API CRM

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Create links with catalog_product.id and catalog_measure.code to the directory when it becomes available

{% endnote %}

{% endif %}

Basic data types are listed in a separate [article](../data-types.md).

In this article, we will discuss the data types and object structure specific to CRM.

## Data Types

#|
|| **Type** | **Descriptions and Values** ||
|| `crm_status` | String identifier of a CRM directory element (for example, `NEW`). Possible values for a specific directory can be obtained using the [crm.status.list](./status/crm-status-list.md) method with a filter by `ENTITY_ID`. ||
|| `crm_lead` | Integer identifier of a CRM lead. Information about the lead can be obtained using the [crm.lead.get](./leads/crm-lead-get.md) method. ||
|| `crm_deal` | Integer identifier of a CRM deal. Information about the deal can be obtained using the [crm.deal.get](./deals/crm-deal-get.md) method. ||
|| `crm_contact` | Integer identifier of a CRM contact. Information about the contact can be obtained using the [crm.contact.get](./contacts/crm-contact-get.md) method. ||
|| `crm_company` | Integer identifier of a CRM company. Information about the company can be obtained using the [crm.company.get](./companies/crm-company-get.md) method. ||
|| `crm_quote` | Integer identifier of a CRM estimate. Information about the estimate can be obtained using the [crm.quote.get](./quote/crm-quote-get.md) method. ||
|| `crm_category` | Integer identifier of a CRM funnel (direction). Information about the funnel (direction) can be obtained using the [crm.category.get](./universal/category/crm-category-get.md) method. ||
|| `crm_entity`| Integer identifier of an element of some CRM object. Fields of this type explicitly contain information about which CRM object they belong to. For example, fields like `parentId{entityTypeId}` contain the identifier of the CRM entity type within their name. Information about the element can be obtained using the [`crm.item.get`](universal/crm-item-get.md) method by passing `entityTypeId` from the field name and `id` from its value. ||
|| `lang_map`  | Object format:
```
{
    lang_1: value_1,
    lang_2: value_2,
    ..
    lang_n: value_n,
}
```

where
`lang_n` - [Language identifier](#lang-ids)
`value_n` - Value for language `lang_n`
||
|| [`crm_item_product_row`](#crm_item_product_row) | Integer identifier of the product item of the CRM object. Product item identifiers can be obtained using the [crm.item.productrow.list](../crm/universal/product-rows/crm-item-productrow-list.md) method. ||
|| [`crm_multifield`](#crm_multifield) | Object describing a "multifield". Multifields are used to store phone numbers, email addresses, and other contact information. In leads, contacts, and companies, fields of this type are `PHONE`, `EMAIL`, `WEB`, and `IM`. ||
|| [`crm_currency`](#crm_currency) | Object describing currency. ||
|| [`crm_currency_localization`](#crm_currency_localization) | Object describing currency localization. ||
|| [`crm_orderentity`](#crm_orderentity) | Object describing the connection between CRM and online store orders. ||
|| [`type`](#type) | Object describing a custom type of CRM object (SPA). ||
|| [`type.relations`](#typerelations) | Object containing relationships to other CRM entities. ||
|| [`relation`](#relation) | Object describing a linked CRM element. ||
|| [`type.linkedUserFields`](#typelinkeduserfields) | Object describing a set of fields in which the SPA should be displayed. ||
|#

## Object Structure

### crm_multifield {#crm_multifield}

#|
|| **Value**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the multifield value. ||
|| **TYPE_ID**
[`string`](../data-types.md) | Type of the multifield. Can take values `PHONE`, `EMAIL`, `WEB`, `IM`, `LINK`. ||
|| **VALUE**
[`string`](../data-types.md) | String value of the multifield. ||
|| **VALUE_TYPE**
[`string`](../data-types.md) | Type of the multifield value.
Can take values `WORK`, `MOBILE`, `FAX`, `HOME`, `PAGER`, `MAILING`, `OTHER` for phone,
`WORK`, `HOME`, `MAILING`, `OTHER` for email,
`WORK`, `HOME`, `FACEBOOK`, `LIVEJOURNAL`, `TWITTER`, `OTHER` for website,
`FACEBOOK`, `TELEGRAM`, `SKYPE`, `VIBER`, `INSTAGRAM`, `BITRIX24`, `OPENLINE`, `IMOL`, `ICQ`, `MSN`, `JABBER`, `OTHER` for messenger. ||
|#

### crm_item_product_row

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the product item. ||
|| **ownerId**
[`integer`](../data-types.md) | Identifier of the CRM object. ||
|| **ownerType**
[`string`](../data-types.md) | Identifier of the [`CRM object type`](#tip-obuekta-crm). ||
|| **productId**
`catalog_product.id` | Identifier of the product from the catalog. ||
|| **productName**
[`string`](../data-types.md) | Name of the product in the product item. ||
|| **price**
[`double`](../data-types.md) | Price per unit of the product item, including discounts and taxes. ||
|| **priceAccount**
[`double`](../data-types.md) | Price per unit of the product item, including discounts and taxes, converted to the reporting currency. ||
|| **priceExclusive**
[`double`](../data-types.md) | Price per unit of the product item, including discounts but excluding taxes. ||
|| **priceNetto**
[`double`](../data-types.md) | Price per unit of the product item, excluding discounts and taxes. ||
|| **priceBrutto**
[`double`](../data-types.md) | Price per unit of the product item, including taxes but excluding discounts. ||
|| **quantity**
[`double`](../data-types.md) | Quantity of the product. ||
|| **discountTypeId**
[`integer`](../data-types.md) | Type of discount.
Possible values:
- `1` — absolute value
- `2` — percentage value. ||
|| **discountRate**
[`double`](../data-types.md) | Discount value in percentage. ||
|| **discountSum**
[`double`](../data-types.md) | Absolute discount value. ||
|| **taxRate**
[`double`](../data-types.md) | Tax rate in percentage. ||
|| **taxIncluded**
[`string`](../data-types.md) | Indicator of whether the tax is included in the price.
Possible values:
- `Y` – tax included
- `N` – tax not included. ||
|| **customized**
[`string`](../data-types.md) | Deprecated. ||
|| **measureCode**
`catalog_measure.code` | Unit of measure code. ||
|| **measureName**
[`string`](../data-types.md) | Text representation of the unit of measure (e.g., pcs, kg, m, l, etc.). ||
|| **sort**
[`integer`](../data-types.md) | Sorting. ||
|| **xmlId**
[`string`](../data-types.md) | External identifier of the product item. ||
|| **type**
[`integer`](../data-types.md) | Type of product.
Possible values:
- `1` - Simple product
- `2` - Bundle
- `3` - Product with trade offers
- `4` - Trade offer
- `5` - Trade offer that has no product (not specified or deleted)
- `6` - Specific type, means invalid product with trade offers
- `7` — Service. ||
|| **storeId**
[`integer`](../data-types.md) | Identifier of the inventory. ||
|#

### crm_currency

#|
|| **Value**
`type` | **Description** ||
|| **AMOUNT**
[`double`](../data-types.md) | Exchange rate relative to the base currency.
For the base currency, it is always equal to `1`. Precision — 4 decimal places. ||
|| **AMOUNT_CNT**
[`integer`](../data-types.md) | Nominal value.
For the base currency, it is always equal to `1`. ||
|| **BASE**
[`string`](../data-types.md) | Indicator of whether the currency is base (`Y`/`N`). ||
|| **CURRENCY**
[`string`](../data-types.md) | Currency identifier. Corresponds to ISO 4217 standard. ||
|| **DATE_UPDATE**
[`datetime`](../data-types.md) | Date of last modification. ||
|| **SORT**
[`integer`](../data-types.md) | Sorting. ||
|| **LID**
[`string`](../data-types.md) | Language code for which the [localization parameters](#crm_currency_localization) are returned. ||
|| **DECIMALS**
[`integer`](../data-types.md) | Number of decimal places in the fractional part (localization parameter). ||
|| **DEC_POINT**
[`string`](../data-types.md) | Decimal point for output (localization parameter). ||
|| **FORMAT_STRING**
[`string`](../data-types.md) | Format template (localization parameter). ||
|| **FULL_NAME**
[`string`](../data-types.md) | Name of the currency (localization parameter). ||
|| **THOUSANDS_SEP**
[`string`](../data-types.md) | Thousands separator (localization parameter). ||
|| **LANG**
[`object`](../data-types.md) | Currency localizations.
Object with a list of available localizations in the format `{"lang_1": "value_1", ... "lang_N": "value_N"}`, where `lang_N` — language identifier, and `value` — object of type [crm_currency_localization](#crm_currency_localization).
Language identifier is a string of two Latin letters. Possible values can be found in the [table of language identifiers](#lang-ids). ||
|#

### crm_currency_localization

#|
|| **Value**
`type` | **Description** ||
|| **DECIMALS**
[`integer`](../data-types.md) | Number of decimal places in the fractional part.

Default value — `2`. ||
|| **DEC_POINT**
[`string`](../data-types.md) | Decimal point for output.

Default value — `.` (dot symbol). ||
|| **FORMAT_STRING**
[`string`](../data-types.md) | Format template. Must contain the symbol # — instead, the price value will be substituted.

Default value — `#`.

Examples of templates for the amount 1000:
- `$ #` — $ 1000
- `# rub.` — 1000 rub.
- `€ #` — € 1000. ||
|| **FULL_NAME**
[`string`](../data-types.md) | Name of the currency.

Default value — [`crm_currency.CURRENCY`](#crm_currency). ||
|| **HIDE_ZERO**
[`string`](../data-types.md) | Indicator of whether to hide insignificant zeros (`Y`/`N`).

Default value — `N`. ||
|| **THOUSANDS_SEP**
[`string`](../data-types.md) | Thousands separator.

Default value — ` ` (space). ||
|| **THOUSANDS_VARIANT**
[`string`](../data-types.md) | Code for the thousands separator.

Default value — `S`.

When creating or modifying localization, if a value is specified for the `THOUSANDS_VARIANT` field, the value in `THOUSANDS_SEP` will be ignored and replaced according to the list:
- `N` — empty, no thousands separator. Example: 12345678
- `C` — comma. Example: 12,345,678
- `D` — dot. Example: 12.345.678
- `S` — space. Example: 12 345 678
- `B` — non-breaking space. Example: 12 345 678
    This differs from the previous option in that when line breaks occur, the result is not split into parts.

If a thousands separator is not needed, the `THOUSANDS_VARIANT` field must be explicitly passed with the value `N`. An empty string in the `THOUSANDS_SEP` field is not allowed. ||
|#

### crm_orderentity

#|
|| **Value**
`type` | **Description** ||
|| **OWNER_ID**
[`integer`](../data-types.md) | Identifier of the CRM object. ||
|| **OWNER_TYPE_ID**
[`integer`](../data-types.md) | Identifier of the [CRM object type](#object_type). ||
|| **ORDER_ID**
[`sale_order.id`](../sale/data-types.md#sale_order) | Identifier of the order. ||
|#

### type

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md)  | Identifier of the SPA. ||
|| **title**
[`string`](../data-types.md) | Title of the SPA. ||
|| **code** 
[`string`](../data-types.md) | Symbolic code. ||
|| **createdBy**
[`integer`](../data-types.md)  | Identifier of the user who created this SPA. ||
|| **entityTypeId**
[`integer`](../data-types.md)  | Identifier of the entity type. ||
|| **isCategoriesEnabled**
[`boolean`](../data-types.md)  | Are custom funnels and sales tunnels enabled? ||
|| **isStagesEnabled**    
[`boolean`](../data-types.md)  | Is the use of custom stages and Kanban enabled? ||
|| **isBeginCloseDatesEnabled**
[`boolean`](../data-types.md)  | Are the **Start Date** and **End Date** fields enabled? ||
|| **isClientEnabled**    
[`boolean`](../data-types.md)  | Is the **Client** field enabled? ||
|| **isUseInUserfieldEnabled** 
[`boolean`](../data-types.md)  | Is the use of the SPA in the user field enabled? ||
|| **isLinkWithProductsEnabled**
[`boolean`](../data-types.md)  | Is the linking of catalog products enabled? ||
|| **isMycompanyEnabled** 
[`boolean`](../data-types.md)  | Is the **Your Company Details** field enabled? ||
|| **isDocumentsEnabled** 
[`boolean`](../data-types.md)  | Is document printing enabled? ||
|| **isSourceEnabled**    
[`boolean`](../data-types.md)  | Are the **Source** and **Additional Information about Source** fields enabled? ||
|| **isObserversEnabled** 
[`boolean`](../data-types.md)  | Is the **Observers** field enabled? ||
|| **isRecyclebinEnabled**
[`boolean`](../data-types.md)  | Is the use of the recycle bin enabled? ||
|| **isAutomationEnabled**
[`boolean`](../data-types.md)  | Are automation rules and triggers enabled? ||
|| **isBizProcEnabled**   
[`boolean`](../data-types.md)  | Is the use of the business process designer enabled? ||
|| **isSetOpenPermissions**    
[`boolean`](../data-types.md)  | Should new funnels be available to everyone? ||
|| **isPaymentsEnabled**  
[`boolean`](../data-types.md)  | System field indicating whether payment capability is enabled. ||
|| **isCountersEnabled**  
[`boolean`](../data-types.md)  | System field indicating whether counters are enabled. ||
|| **createdTime** 
[`datetime`](../data-types.md) | System field indicating the creation time of the SPA. ||
|| **updatedTime** 
[`datetime`](../data-types.md) | System field indicating the time of modification of this SPA. ||
|| **updatedBy**
[`integer`](../data-types.md)  | Identifier of the user who modified this SPA. ||
|| **relations**
[`object`](../data-types.md)   | Object containing relationships to other CRM entities. ||
|| **linkedUserFields**   
[`object`](../data-types.md)   | Set of fields in which this SPA should be displayed. ||
|| **customSections**
[`array`](../data-types.md)    | List of all digital workplaces.

This parameter is deprecated. To work with digital workplaces, use the methods [`crm.automatedsolution.*`](./automated-solution/index.md). ||
|| **customSectionId**
[`integer`](../data-types.md)  | Identifier of the digital workplace.

This parameter is deprecated. To work with digital workplaces, use the methods [`crm.automatedsolution.*`](./automated-solution/index.md). ||
|#

### type.relations

#|
|| **Value**
`type` | **Description** ||
|| **parent** 
`relation[]`  | CRM elements that will be linked to this SPA. ||
|| **child**  
`relation[]`  | CRM elements to which this SPA will be linked. ||
|#

#### relation

#|
|| **Value**
`type` | **Description** ||
|| **entityTypeId**     
[`integer`](../data-types.md)  | Identifier of the [system](./universal/index.md) or [user-defined type](./universal/user-defined-object-types/index.md) of CRM entity. ||
|| **isChildrenListEnabled**
[`boolean`](../data-types.md)  | Should the linked element be added to the card? ||
|| **isPredefined**        
[`boolean`](../data-types.md)  | Is this relationship predefined (system)? ||
|#

### type.linkedUserFields

#|
|| **Value**
`type` | **Description** ||
|| **CALENDAR_EVENT\|UF_CRM_CAL_EVENT**
[`boolean`](../data-types.md) | Calendar event. ||
|| **TASKS_TASK\|UF_CRM_TASK**
[`boolean`](../data-types.md) | Tasks. ||
|| **TASKS_TASK_TEMPLATE\|UF_CRM_TASK**
[`boolean`](../data-types.md) | Task templates. ||
|#

## Language Identifiers for Bitrix24 {#lang-ids}

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
|| `la` | Spanish ||
|| `ms` | Malay ||
|| `pl` | Polish ||
|| `ru` | Russian ||
|| `sc` | Chinese ||
|| `tc` | Chinese (Taiwan) ||
|| `th` | Thai ||
|| `tr` | Turkish ||
|| `ua` | Ukrainian ||
|| `vn` | Vietnamese ||
|#

## Objects Used in Responses

### Description of a Single Field crm_rest_field_description {#crm_rest_field_description}

#|
|| **Name** 
`type` | **Description** ||
|| **type**
[`string`](../data-types.md)  | Type of the field. ||
|| **isRequired**
[`boolean`](../data-types.md) | Is the field required? ||
|| **isReadOnly**
[`boolean`](../data-types.md) | Is the field read-only? ||
|| **isImmutable**
[`boolean`](../data-types.md) | Indicator of whether the field value can only be filled once when creating a new element. ||
|| **isMultiple**
[`boolean`](../data-types.md) | Indicator of whether the field is multiple. If true, values in the field are passed as an array. ||
|| **isDynamic**
[`boolean`](../data-types.md) | Is the field user-defined? ||
|| **title**
[`string`](../data-types.md)  | Name of the field. ||
|| **upperName**
[`string`](../data-types.md)  | Name of the field in uppercase. ||
|#

### Description of User Field Type Address {#crm_user_field_address}

The user field of type "Address" stores data in a single line. The table provides a description of the components of this line.
A detailed description of the components of the address can be found in the article [About Addresses](./requisites/addresses/index.md).

#|
|| **Name** 
`type` | **Description** | **Example** ||
|| **ADDRESS_1**
[`string`](../data-types.md)  | Street, house number | Small Znamenka Lane 7/10 b2 ||
|| **ADDRESS_2**
[`string`](../data-types.md) | Apartment, office, room, floor | 5 ||
|| **POSTAL_CODE**
[`string`](../data-types.md) | Postal code | 119019 ||
|| **CITY**
[`string`](../data-types.md) | Settlement | New York ||
|| **REGION**
[`string`](../data-types.md) | District | Arbat District ||
|| **PROVINCE**
[`string`](../data-types.md) | Region | New York ||
|| **COUNTRY**
[`string`](../data-types.md) | Country | United States ||
|| **LATITUDE**
[`string`](../data-types.md)  | Latitude coordinates | 55.748289 ||
|| **LONGITUDE**
[`string`](../data-types.md)  | Longitude coordinates | 37.60504 ||
|| **LOC_ADDR_ID**
[`string`](../data-types.md)  | Location address identifier | 10 ||
|#

## CRM Object Type {#object_type}

#|
|| **Object Type** | **Numeric Identifier of Type**
`entityTypeId` | **Symbolic Code of Type**
`entityTypeName` | **Short Symbolic Code of Type**
`entityTypeAbbr` | **Type of User Field Object**
`userFieldEntityId` ||
|| Lead | 1 | LEAD | L | CRM_LEAD ||
|| Deal | 2 | DEAL | D | CRM_DEAL ||
|| Contact | 3 | CONTACT | C | CRM_CONTACT ||
|| Company | 4 | COMPANY | CO| CRM_COMPANY ||
|| Invoice (old) | 5 | INVOICE | I | CRM_INVOICE ||
|| Invoice (new) | 31 | SMART_INVOICE | SI | CRM_SMART_INVOICE ||
|| Estimate | 7 | QUOTE | Q | CRM_QUOTE ||
|| Requisite | 8 | REQUISITE | RQ | CRM_REQUISITE ||
|| Order | 14 | ORDER | O | ORDER ||
|| SPA | 128 | DYNAMIC_128 | T80 | CRM_1 ||
|#

> The table describes the SPA with type identifier 128 and identifier 1.
>
> The identifiers of SPA types are in the range from 128 to 191 (inclusive), or have a value greater than 1030 (inclusive) and are even.
>
> The identifiers of SPA types that have been deleted to the recycle bin are in the range from 192 to 255 (inclusive), or have a value greater than 1030 and are odd.
>
> To determine the prefix of the SPA, it is necessary to:
> - convert the identifier from decimal to hexadecimal. In the case of an identifier equal to 128, we get the value 80.
> - take the obtained value in lowercase and prepend the Latin letter T to the obtained value. Thus, we get the prefix — T80.