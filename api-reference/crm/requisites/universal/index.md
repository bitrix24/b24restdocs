# About Universal Requisites

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- from Sergey's file: what fields they consist of, what they are needed for

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Company requisites are details that allow for the precise identification of an organization. Every officially registered company has a set of such data, including: name, legal address, TIN, OGRN, KPP, OKPO code, OKVED code, and others. 
We explain more in the article [What are your company's requisites](https://helpdesk.bitrix24.com/open/16030918/)

## Requisite Fields {#fields}

#|
|| **Name**
`type` | **Description** | **Read** | **Write** | **Required** | **Immutable** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the requisite, can be obtained using the method [crm.requisite.list](./crm-requisite-list.md). Created automatically and is unique within the account. | Yes | No | No | No ||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the parent entity type.

Currently, this can only be:
- `3` — contact
- `4` — company

Identifiers of all CRM entity types can be obtained from the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md)

 | Yes | Yes | Yes | Yes ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Identifier of the parent entity (contact or company).

The identifier can be obtained using the method [crm.company.list](../../companies/crm-company-list.md) for companies and the method [crm.contact.list](../../contacts/crm-contact-list.md) for contacts | Yes | Yes | Yes | Yes ||
|| **PRESET_ID***
[`integer`](../../../data-types.md) | Identifier of the requisites template.

Template identifiers can be obtained using the method [crm.requisite.preset.list](../presets/crm-requisite-preset-list.md) | Yes | Yes | Yes | Yes ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Creation date | Yes | No | No | No ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date | Yes | No | No | No ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite | Yes | No | No | No ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the requisite | Yes | No | No | No ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the requisite | Yes | Yes | Yes | No ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the requisite | Yes | Yes | No | No ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key, used for exchange operations.

Identifier of the object in the external information database.

The purpose of the field may change by the final developer | Yes | Yes | No | No ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the external information database.

The purpose of the field may change by the final developer | Yes | Yes | No | No ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status.

Values `Y` or `N` are used.

Currently, the field does not actually affect anything | Yes | Yes | No | No ||
|| **ADDRESS_ONLY**
[`char`](../../../data-types.md) | Status indicator when the requisite is used only for storing the address.

Values `Y` or `N` are used. When `Y`, the requisites are not displayed in the entity detail form, but the address is shown | Yes | Yes | No | No ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting. Order in the list of entity requisites when there are multiple | Yes | Yes | No | No ||
|| **RQ_NAME**
[`string`](../../../data-types.md) | Full name | Yes | Yes | No | No ||
|| **RQ_FIRST_NAME**
[`string`](../../../data-types.md) | First name | Yes | Yes | No | No ||
|| **RQ_LAST_NAME**
[`string`](../../../data-types.md) | Last name | Yes | Yes | No | No ||
|| **RQ_SECOND_NAME**
[`string`](../../../data-types.md) | Middle name | Yes | Yes | No | No ||
|| **RQ_COMPANY_ID**
[`string`](../../../data-types.md) | Identifier of the organization | Yes | Yes | No | No ||
|| **RQ_COMPANY_NAME**
[`string`](../../../data-types.md) | Short name of the organization | Yes | Yes | No | No ||
|| **RQ_COMPANY_FULL_NAME**
[`string`](../../../data-types.md) | Full name of the organization | Yes | Yes | No | No ||
|| **RQ_COMPANY_REG_DATE**
[`string`](../../../data-types.md) | Date of state registration | Yes | Yes | No | No ||
|| **RQ_DIRECTOR**
[`string`](../../../data-types.md) | General Director | Yes | Yes | No | No ||
|| **RQ_ACCOUNTANT**
[`string`](../../../data-types.md) | Chief Accountant | Yes | Yes | No | No ||
|| **RQ_CEO_NAME**
[`string`](../../../data-types.md) | Full name of the first leader | Yes | Yes | No | No ||
|| **RQ_CEO_WORK_POS**
[`string`](../../../data-types.md) | Position of the first leader | Yes | Yes | No | No ||
|| **RQ_CONTACT**
[`string`](../../../data-types.md) | Contact person | Yes | Yes | No | No ||
|| **RQ_EMAIL**
[`string`](../../../data-types.md) | E-Mail | Yes | Yes | No | No ||
|| **RQ_PHONE**
[`string`](../../../data-types.md) | Phone | Yes | Yes | No | No ||
|| **RQ_FAX**
[`string`](../../../data-types.md) | Fax | Yes | Yes | No | No ||
|| **RQ_IDENT_TYPE**
[`crm_status`](../../../data-types.md) | Identification method | Yes | Yes | No | No ||
|| **RQ_IDENT_DOC**
[`string`](../../../data-types.md) | Type of document | Yes | Yes | No | No ||
|| **RQ_IDENT_DOC_SER**
[`string`](../../../data-types.md) | Series | Yes | Yes | No | No ||
|| **RQ_IDENT_DOC_NUM**
[`string`](../../../data-types.md) | Number | Yes | Yes | No | No ||
|| **RQ_IDENT_DOC_PERS_NUM**
[`string`](../../../data-types.md) | Personal number | Yes | Yes | No | No ||
|| **RQ_IDENT_DOC_DATE**
[`string`](../../../data-types.md) | Date of issue | Yes | Yes | No | No ||
|| **RQ_IDENT_DOC_ISSUED_BY**
[`string`](../../../data-types.md) | Issued by | Yes | Yes | No | No ||
|| **RQ_IDENT_DOC_DEP_CODE**
[`string`](../../../data-types.md) | Department code | Yes | Yes | No | No ||
|| **RQ_INN**
[`string`](../../../data-types.md) | TIN | Yes | Yes | No | No ||
|| **RQ_KPP**
[`string`](../../../data-types.md) | KPP | Yes | Yes | No | No ||
|| **RQ_USRLE**
[`string`](../../../data-types.md) | Handelsregisternummer (for country DE) | Yes | Yes | No | No ||
|| **RQ_IFNS**
[`string`](../../../data-types.md) | IFNS | Yes | Yes | No | No ||
|| **RQ_OGRN**
[`string`](../../../data-types.md) | OGRN | Yes | Yes | No | No ||
|| **RQ_OGRNIP**
[`string`](../../../data-types.md) | OGRNIP | Yes | Yes | No | No ||
|| **RQ_OKPO**
[`string`](../../../data-types.md) | OKPO | Yes | Yes | No | No ||
|| **RQ_OKTMO**
[`string`](../../../data-types.md) | OKTMO | Yes | Yes | No | No ||
|| **RQ_OKVED**
[`string`](../../../data-types.md) | OKVED | Yes | Yes | No | No ||
|| **RQ_EDRPOU**
[`string`](../../../data-types.md) | EDRPOU | Yes | Yes | No | No ||
|| **RQ_DRFO**
[`string`](../../../data-types.md) | DRFO | Yes | Yes | No | No ||
|| **RQ_KBE**
[`string`](../../../data-types.md) | KBE | Yes | Yes | No | No ||
|| **RQ_IIN**
[`string`](../../../data-types.md) | IIN | Yes | Yes | No | No ||
|| **RQ_BIN**
[`string`](../../../data-types.md) | BIN | Yes | Yes | No | No ||
|| **RQ_ST_CERT_SER**
[`string`](../../../data-types.md) | Series of the state registration certificate | Yes | Yes | No | No ||
|| **RQ_ST_CERT_NUM**
[`string`](../../../data-types.md) | Number of the state registration certificate | Yes | Yes | No | No ||
|| **RQ_ST_CERT_DATE**
[`string`](../../../data-types.md) | Date of the state registration certificate | Yes | Yes | No | No ||
|| **RQ_VAT_PAYER**
[`char`](../../../data-types.md) | VAT payer (for country UA).

Values `Y` or `N` are used | Yes | Yes | No | No ||
|| **RQ_VAT_ID**
[`string`](../../../data-types.md) | VAT ID (identification number of the VAT payer) | Yes | Yes | No | No ||
|| **RQ_VAT_CERT_SER**
[`string`](../../../data-types.md) | Series of the VAT certificate | Yes | Yes | No | No ||
|| **RQ_VAT_CERT_NUM**
[`string`](../../../data-types.md) | Number of the VAT certificate | Yes | Yes | No | No ||
|| **RQ_VAT_CERT_DATE**
[`string`](../../../data-types.md) | Date of the VAT certificate | Yes | Yes | No | No ||
|| **RQ_RESIDENCE_COUNTRY**
[`string`](../../../data-types.md) | Country of residence | Yes | Yes | No | No ||
|| **RQ_BASE_DOC**
[`string`](../../../data-types.md) | Basis of the action | Yes | Yes | No | No ||
|| **RQ_REGON**
[`string`](../../../data-types.md) | REGON (for country PL) | Yes | Yes | No | No ||
|| **RQ_KRS**
[`string`](../../../data-types.md) | KRS (for country PL) | Yes | Yes | No | No ||
|| **RQ_PESEL**
[`string`](../../../data-types.md) | PESEL (for country PL) | Yes | Yes | No | No ||
|| **RQ_LEGAL_FORM**
[`string`](../../../data-types.md) | Legal form (for country FR) | Yes | Yes | No | No ||
|| **RQ_SIRET**
[`string`](../../../data-types.md) | Siret number (for country FR) | Yes | Yes | No | No ||
|| **RQ_SIREN**
[`string`](../../../data-types.md) | Siren number (for country FR) | Yes | Yes | No | No ||
|| **RQ_CAPITAL**
[`string`](../../../data-types.md) | Share capital (for country FR) | Yes | Yes | No | No ||
|| **RQ_RCS**
[`string`](../../../data-types.md) | RCS (for country FR) | Yes | Yes | No | No ||
|| **RQ_CNPJ**
[`string`](../../../data-types.md) | CNPJ (for country BR) | Yes | Yes | No | No ||
|| **RQ_STATE_REG**
[`string`](../../../data-types.md) | State Registration (IE) (for country BR) | Yes | Yes | No | No ||
|| **RQ_MNPL_REG**
[`string`](../../../data-types.md) | Municipal Registration (IM) (for country BR) | Yes | Yes | No | No ||
|| **RQ_CPF**
[`string`](../../../data-types.md) | CPF (for country BR) | Yes | Yes | No | No ||
|| **UF_CRM_...** | Custom fields. For example, `UF_CRM_1694526604`.

Requisites can have a set of custom fields with types: `string`, `boolean`, `double`, `datetime`.

You can add a custom field to requisites using the method [crm.requisite.userfield.add](../user-fields/crm-requisite-userfield-add.md) | Yes | Yes | No | No ||
|#

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [crm.requisite.add](./crm-requisite-add.md) | Creates a new requisite ||
|| [crm.requisite.update](./crm-requisite-update.md) | Updates an existing requisite ||
|| [crm.requisite.get](./crm-requisite-get.md) | Returns the requisite by identifier ||
|| [crm.requisite.list](./crm-requisite-list.md) | Returns a list of requisites by filter ||
|| [crm.requisite.delete](./crm-requisite-delete.md) | Deletes the requisite and all related objects ||
|| [crm.requisite.fields](./crm-requisite-fields.md) | Returns the description of requisite fields ||
|#