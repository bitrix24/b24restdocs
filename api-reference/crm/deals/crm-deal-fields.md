# Get Deal Fields crm.deal.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (in other languages)
- success response is missing
- error response is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.fields` returns the description of [deal fields](./crm-deal-add.md), including [custom fields](./user-defined-fields/crm-deal-userfield-add.md).

No parameters.

## Example

```js
BX24.callMethod(
    "crm.deal.fields",
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```

{% include [Example notes](../../../_includes/examples.md) %}

## Returned Data

#|
|| **Field** / **Type** | **Description** ||
|| **ADDITIONAL_INFO**
[`string`](../../data-types.md) | Additional information. ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Linked to user by ID. ||
|| **BANK_DETAIL_ID**
[`integer`](../../data-types.md) | Bank detail ID. Accepted but not returned. This parameter is passed to the function [crm.requisite.link.register](.) automatically upon successful addition/update of a deal with this deal's identifier. ||
|| **BEGINDATE**
[`date`](../../data-types.md) | Start date. ||
|| **CATEGORY_ID**
[`crm_category`](../../data-types.md) | Identifier of the direction. Immutable. If this field is not passed when creating a deal, the deal will be created in the general direction. ||
|| **CLOSED**
[`char`](../../data-types.md) | Closed. ||
|| **CLOSEDATE**
[`date`](../../data-types.md) | Close date. ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comments. ||
|| **COMPANY_ID**
[`crm_company`](../../data-types.md) | Identifier of the linked company. ||
|| **CONTACT_ID**
[`crm_contact`](../../data-types.md) | Identifier of the linked contact. Deprecated. Kept for compatibility. ||
|| **CONTACT_IDS**
[`crm_contact`](../../data-types.md) | Identifier of the linked contact |  | Multiple. When using [crm.deal.update](./crm-deal-update.md) and [crm.deal.add](./crm-deal-add.md), an array of contacts can be submitted. This field is not present in [crm.deal.list](./crm-deal-list.md) and [crm.deal.get](./crm-deal-get.md) methods, and [crm.deal.contact.items.get](./contacts/crm-deal-contact-items-get.md) should be used to get the list of contacts. To clear the field, use [crm.deal.contact.items.delete](./contacts/crm-deal-contact-items-delete.md), to replace the value use [crm.deal.contact.items.set](./contacts/crm-deal-contact-items-set.md). ||
|| **CREATED_BY_ID**
[`user`](../../data-types.md) | Created by user. Read-only. ||
|| **CURRENCY_ID**
[`crm_currency`](../../data-types.md) | Currency identifier of the deal. ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Creation date. Read-only. ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Modification date. Read-only. ||
|| **ID**
[`integer`](../../data-types.md) | Deal identifier. Read-only. ||
|| **IS_NEW**
[`char`](../../data-types.md) | Flag for a new deal (i.e., the deal is in the first stage). ||
|| **IS_RECURRING**
[`char`](../../data-types.md) | Flag for a recurring deal template (if set to Y, then this is not a deal, but a template). ||
|| **IS_RETURN_CUSTOMER**
[`char`](../../data-types.md) | Indicator of a returning lead. ||
|| **LEAD_ID**
[`crm_lead`](../../data-types.md) | Identifier of the linked lead. Read-only. ||
|| **LOCATION_ID**
[`location`](../../data-types.md) | Client's location. Service field, not recommended for use. ||
|| **MODIFY_BY_ID**
[`user`](../../data-types.md) | Identifier of the author of the last modification. Read-only. ||
|| **MOVED_BY_ID**
[`user`](../../data-types.md) | Identifier of the author who moved the item to the current stage. Read-only. ||
|| **MOVED_TIME**
[`datetime`](../../data-types.md) | Date of moving the item to the current stage. Read-only. ||
|| **OPENED**
[`char`](../../data-types.md) | Available to everyone. ||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Amount. ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the data source. Used only for linking to an external source. ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the item in the data source. Used only for linking to an external source. ||
|| **PROBABILITY**
[`integer`](../../data-types.md) | Probability. ||
|| **QUOTE_ID**
[`crm_quote`](../../data-types.md) | Identifier of the quote. Read-only. Deprecated, use the method [crm.quote.list](.) with a filter by deal. ||
|| **REQUISITE_ID** | Identifier of the requisite. Accepted but not returned. This parameter is passed to the function [crm.requisite.link.register](.) automatically upon successful addition/update of a deal with this deal's identifier. ||
|| **STAGE_ID**
[`crm_status`](../../data-types.md) | Identifier of the stage.

```
 NEW // new deal 
 PREPARATION // preparing documents 
 PREPAYMENT_INVOICE // sending invoice 
 EXECUTING // in the process of execution 
 FINAL_INVOICE // final invoice 
```

  (P - value in STAGE_SEMANTIC_ID) 


```
  WON // won 
```

  (S - value in STAGE_SEMANTIC_ID) 


```
  LOST // lost, analysis of reasons not required 
  APOLOGY // lost, analysis of reasons required 
```


  (F - value in STAGE_SEMANTIC_ID) ||
|| **STAGE_SEMANTIC_ID**
[`string`](../../data-types.md) | Name. Read-only, which somewhat generalizes the values of the deal identifier STAGE_ID. (See values above.) ||
|| **SOURCE_ID**
[`string`](../../data-types.md) | Identifier of the source. Defines the source of the deal (callback, advertisement, e-mail, etc.). The list of possible identifiers can be retrieved using the REST method [crm.status.list](.) with the filter `filter[ENTITY_ID]=SOURCE` ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the source. Text field. ||
|| **TAX_VALUE**
[`double`](../../data-types.md) | Tax rate. ||
|| **TITLE**^*^
[`string`](../../data-types.md) | Title. ||
|| **TYPE_ID**
[`crm_status`](../../data-types.md) | Type of deal. Used only for linking to an external source. ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Identifier of the advertising campaign. ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads. ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Type of traffic. CPC (ads), CPM (banners) ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system. Yandex-Direct, Google-Adwords, and others. ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Search term of the campaign. For example, keywords for contextual advertising. ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}