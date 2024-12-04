# Get fields of the estimate crm.quote.fields

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing examples (there should be three examples - curl, js, php)
- missing error response
- missing success response

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.fields` returns the description of fields for the [estimate](./crm-quote-add.md), including [custom](./crm-quote-user-field-add.md) fields.

No parameters required.

## Example

{% list tabs %}

- JS

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

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Fields

#|
|| **Field** / **Type** | **Description** | **Note** ||
|| **ASSIGNED_BY_ID** 
[`user`](../../data-types.md) | Linked to user by ID | ||
|| **BEGINDATA** 
[`date`](../../data-types.md) | Date of issuance | ||
|| **CLIENT_ADDR** 
[`string`](../../data-types.md) | Contact address | ||
|| **CLIENT_CONTACT** 
[`string`](../../data-types.md) | Contact | Deprecated. Retained for compatibility. ||
|| **CLIENT_EMAIL** 
[`string`](../../data-types.md) | Contact's email address | Deprecated. Retained for compatibility. ||
|| **CLIENT_PHONE** 
[`char`](../../data-types.md) | Phone field validation | Read-only. Deprecated. Retained for compatibility. ||
|| **CLIENT_TITLE** 
[`string`](../../data-types.md) | Client name | Deprecated. Retained for compatibility. ||
|| **CLIENT_TPA_ID** 
[`string`](../../data-types.md) | Client's TIN | Deprecated. Retained for compatibility. ||
|| **CLIENT_TP_ID** 
[`string`](../../data-types.md) | Client's VAT ID | Deprecated. Retained for compatibility. ||
|| **CLOSED** 
[`char`](../../data-types.md) | Completed | ||
|| **CLOSEDATA** 
[`date`](../../data-types.md) | Date of completion | ||
|| **COMMENTS** 
[`string`](../../data-types.md) | Manager's comment | ||
|| **COMPANY_ID** 
[`crm_company`](../../data-types.md) | Linked to company | ||
|| **CONTACT_ID** 
[`crm_contact`](../../data-types.md) | Linked to contact | Deprecated. Retained for compatibility. ||
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
[`integer`](../../data-types.md) | Payer type identifier | ||
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
[`string`](../../data-types.md) | Advertising campaign designation | ||
|| **UTM_CONTENT** 
[`string`](../../data-types.md) | Campaign content | For example, for contextual ads. ||
|| **UTM_MEDIUM** 
[`string`](../../data-types.md) | Traffic type | CPC (ads), CPM (banners) ||
|| **UTM_SOURCE** 
[`string`](../../data-types.md) | Advertising system | Google-Adwords and others. ||
|| **UTM_TERM** 
[`string`](../../data-types.md) | Campaign search term | For example, keywords for contextual advertising. ||
|#