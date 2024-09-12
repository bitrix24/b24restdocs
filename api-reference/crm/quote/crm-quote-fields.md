# Get Fields of the Estimate crm.quote.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.fields` returns the description of the fields of the [estimate](./crm-quote-add.md), including [custom fields](./crm-quote-user-field-add.md).

No parameters required.

## Example

```js
BX24.callMethod(
    "crm.quote.fields",
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

{% include [Note on examples](../../../_includes/examples.md) %}

## Fields

#|
|| **Field** / **Type** | **Description** | **Note** ||
|| **ASSIGNED_BY_ID** 
[`user`](../../data-types.md) | Linked to user by ID | ||
|| **BEGINDATA** 
[`date`](../../data-types.md) | Date of issue | ||
|| **CLIENT_ADDR** 
[`string`](../../data-types.md) | Contact address | ||
|| **CLIENT_CONTACT** 
[`string`](../../data-types.md) | Contact | Deprecated. Kept for compatibility. ||
|| **CLIENT_EMAIL** 
[`string`](../../data-types.md) | Contact's e-mail address | Deprecated. Kept for compatibility. ||
|| **CLIENT_PHONE** 
[`char`](../../data-types.md) | Phone field validation | Read-only. Deprecated. Kept for compatibility. ||
|| **CLIENT_TITLE** 
[`string`](../../data-types.md) | Client title | Deprecated. Kept for compatibility. ||
|| **CLIENT_TPA_ID** 
[`string`](../../data-types.md) | Client's KPP | Deprecated. Kept for compatibility. ||
|| **CLIENT_TP_ID** 
[`string`](../../data-types.md) | Client's INN | Deprecated. Kept for compatibility. ||
|| **CLOSED** 
[`char`](../../data-types.md) | Completed | ||
|| **CLOSEDATA** 
[`date`](../../data-types.md) | Date of completion | ||
|| **COMMENTS** 
[`string`](../../data-types.md) | Manager's comment | ||
|| **COMPANY_ID** 
[`crm_company`](../../data-types.md) | Linked to company | ||
|| **CONTACT_ID** 
[`crm_contact`](../../data-types.md) | Linked to contact | Deprecated. Kept for compatibility. ||
|| **CONTACT_IDS** 
[`crm_contact`](../../data-types.md) | Linked to multiple contacts | Multiple ||
|| **CONTENT** 
[`string`](../../data-types.md) | Content of the estimate | ||
|| **CREATED_BY_ID** 
[`user`](../../data-types.md) | Created by | Read-only ||
|| **CURRENCY_ID** 
[`crm_currency`](../../data-types.md) | Currency of the estimate | ||
|| **DATE_CREATE** 
[`datetime`](../../data-types.md) | Creation date | Read-only ||
|| **DATE_MODIFY** 
[`datetime`](../../data-types.md) | Modification date | Read-only ||
|| **DEAL_ID** 
[`crm_deal`](../../data-types.md) | Deal linked to the estimate | ||
|| **ID** 
[`integer`](../../data-types.md) | Identifier of the estimate | Read-only ||
|| **LEAD_ID** 
[`crm_lead`](../../data-types.md) | Lead linked to the estimate | ||
|| **LOCATION_ID** 
[`location`](../../data-types.md) | Location | ||
|| **MODIFY_BY_ID** 
[`user`](../../data-types.md) | Identifier of the last modification author | Read-only ||
|| **MYCOMPANY_ID** 
[`crm_company`](../../data-types.md) | Identifier of the company making the estimate | ||
|| **OPENED** 
[`char`](../../data-types.md) | Available to everyone | ||
|| **OPPORTUNITY** 
[`double`](../../data-types.md) | Amount | ||
|| **PERSON_TYPE_ID** 
[`integer`](../../data-types.md) | Identifier of the payer type | ||
|| **QUOTE_NUMBER** 
[`string`](../../data-types.md) | Estimate number | Read-only ||
|| **STATUS_ID** 
[`crm_status`](../../data-types.md) | Status | Status from the directory ||
|| **TAX_VALUE** 
[`double`](../../data-types.md) | Amount | ||
|| **TERMS** 
[`string`](../../data-types.md) | Terms | ||
|| **TITLE** 
[`string`](../../data-types.md) | Title | Required ||
|| **UTM_CAMPAIGN** 
[`string`](../../data-types.md) | Designation of the advertising campaign | ||
|| **UTM_CONTENT** 
[`string`](../../data-types.md) | Content of the campaign | For example, for contextual ads. ||
|| **UTM_MEDIUM** 
[`string`](../../data-types.md) | Type of traffic | CPC (ads), CPM (banners) ||
|| **UTM_SOURCE** 
[`string`](../../data-types.md) | Advertising system | Yandex-Direct, Google-Adwords, and others. ||
|| **UTM_TERM** 
[`string`](../../data-types.md) | Search condition of the campaign | For example, keywords for contextual advertising. ||
|#