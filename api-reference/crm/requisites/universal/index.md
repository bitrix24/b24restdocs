# Universal Company Details CRM: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- from Sergey's file: what fields they consist of, what they are needed for

{% endnote %}

{% endif %}

Company requisites are details that allow for the precise identification of an organization. Every officially registered company has a set of such data, including: name, legal address, Taxpayer ID, OGRN, KPP, OKPO code, OKVED code, and others.
> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [How to Add Your Company Details](https://helpdesk.bitrix24.com/open/24353440/)

## Getting Started

1. Determine the parent object: contact or company
2. Retrieve the requisite preset identifier using the [crm.requisite.preset.list](../presets/crm-requisite-preset-list.md) method
3. Create a requisite using the [crm.requisite.add](./crm-requisite-add.md) method
4. Add an address via [address](../addresses/index.md) methods if the requisite requires a legal or street address
5. Add banking details via [banking details](../bank-detail/index.md) methods if the requisite is used for payment documents
6. Retrieve or update a requisite using the [crm.requisite.get](./crm-requisite-get.md) and [crm.requisite.update](./crm-requisite-update.md) methods

## Requisite Identifiers

- `ID` — requisite identifier. It is returned by the [crm.requisite.add](./crm-requisite-add.md), [crm.requisite.get](./crm-requisite-get.md), and [crm.requisite.list](./crm-requisite-list.md) methods
- `ENTITY_TYPE_ID` — parent object type. For a contact, pass `3`; for a company, pass `4`. All values are provided by the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method
- `ENTITY_ID` — parent contact or company identifier. This can be retrieved using the [crm.contact.list](../../contacts/crm-contact-list.md) or [crm.company.list](../../companies/crm-company-list.md) methods
- `PRESET_ID` — requisite preset identifier. This can be retrieved using the [crm.requisite.preset.list](../presets/crm-requisite-preset-list.md) method

## Requisite Fields {#fields}

Required fields are marked with `*`.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Requisite identifier, which can be obtained using the [crm.requisite.list](./crm-requisite-list.md) method. It is created automatically and is unique within the portal ||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Parent object type identifier.

Currently, this can only be:
- `3` — contact
- `4` — company

The [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method returns identifiers for all CRM object types

||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Parent object identifier (contact or company).

The identifier can be obtained using the [crm.company.list](../../companies/crm-company-list.md) method for a company and the [crm.contact.list](../../contacts/crm-contact-list.md) method for a contact ||
|| **PRESET_ID***
[`integer`](../../../data-types.md) | Identifier of the requisite template.

Template identifiers can be obtained using the method [crm.requisite.preset.list](../presets/crm-requisite-preset-list.md) ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Create date ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who changed the requisite ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the requisite ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key, used for exchange operations.

Identifier of the external information database.

The purpose of the field may change by the final developer ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the external information database.

The purpose of the field may change by the final developer ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status.

Values `Y` or `N` are used.

Currently, the field does not affect anything ||
|| **ADDRESS_ONLY**
[`char`](../../../data-types.md) | A status flag indicating when the requisite is used only for storing an address.

Values `Y` or `N` are used. When the value is `Y`, Company details are not shown in the object card, but the address is displayed ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting. The order in the object's requisite list when there are multiple requisites ||
|| **RQ_NAME**
[`string`](../../../data-types.md) | Full Name ||
|| **RQ_FIRST_NAME**
[`string`](../../../data-types.md) | First Name ||
|| **RQ_LAST_NAME**
[`string`](../../../data-types.md) | Last Name ||
|| **RQ_SECOND_NAME**
[`string`](../../../data-types.md) | Middle Name ||
|| **RQ_COMPANY_ID**
[`string`](../../../data-types.md) | Identifier of the organization ||
|| **RQ_COMPANY_NAME**
[`string`](../../../data-types.md) | Short name of the organization ||
|| **RQ_COMPANY_FULL_NAME**
[`string`](../../../data-types.md) | Full name of the organization ||
|| **RQ_COMPANY_REG_DATE**
[`string`](../../../data-types.md) | Date of state registration ||
|| **RQ_DIRECTOR**
[`string`](../../../data-types.md) | General director ||
|| **RQ_ACCOUNTANT**
[`string`](../../../data-types.md) | Chief accountant ||
|| **RQ_CEO_NAME**
[`string`](../../../data-types.md) | Full name of the first leader ||
|| **RQ_CEO_WORK_POS**
[`string`](../../../data-types.md) | Position of the first leader ||
|| **RQ_CONTACT**
[`string`](../../../data-types.md) | Contact person ||
|| **RQ_EMAIL**
[`string`](../../../data-types.md) | E-Mail ||
|| **RQ_PHONE**
[`string`](../../../data-types.md) | Phone ||
|| **RQ_FAX**
[`string`](../../../data-types.md) | Fax ||
|| **RQ_IDENT_TYPE**
[`crm_status`](../../../data-types.md) | Method of identification ||
|| **RQ_IDENT_DOC**
[`string`](../../../data-types.md) | Type of document ||
|| **RQ_IDENT_DOC_SER**
[`string`](../../../data-types.md) | Series ||
|| **RQ_IDENT_DOC_NUM**
[`string`](../../../data-types.md) | Number ||
|| **RQ_IDENT_DOC_PERS_NUM**
[`string`](../../../data-types.md) | Personal number ||
|| **RQ_IDENT_DOC_DATE**
[`string`](../../../data-types.md) | Date of issue ||
|| **RQ_IDENT_DOC_ISSUED_BY**
[`string`](../../../data-types.md) | Issued by ||
|| **RQ_IDENT_DOC_DEP_CODE**
[`string`](../../../data-types.md) | Department code ||
|| **RQ_INN**
[`string`](../../../data-types.md) | TIN ||
|| **RQ_KPP**
[`string`](../../../data-types.md) | KPP ||
|| **RQ_USRLE**
[`string`](../../../data-types.md) | Handelsregisternummer (for country DE) ||
|| **RQ_IFNS**
[`string`](../../../data-types.md) | IFNS ||
|| **RQ_OGRN**
[`string`](../../../data-types.md) | OGRN ||
|| **RQ_OGRNIP**
[`string`](../../../data-types.md) | OGRNIP ||
|| **RQ_OKPO**
[`string`](../../../data-types.md) | OKPO ||
|| **RQ_OKTMO**
[`string`](../../../data-types.md) | OKTMO ||
|| **RQ_OKVED**
[`string`](../../../data-types.md) | OKVED ||
|| **RQ_EDRPOU**
[`string`](../../../data-types.md) | EDRPOU ||
|| **RQ_DRFO**
[`string`](../../../data-types.md) | DRFO ||
|| **RQ_KBE**
[`string`](../../../data-types.md) | KBE ||
|| **RQ_IIN**
[`string`](../../../data-types.md) | IIN ||
|| **RQ_BIN**
[`string`](../../../data-types.md) | BIN ||
|| **RQ_ST_CERT_SER**
[`string`](../../../data-types.md) | Series of State Registration Certificate ||
|| **RQ_ST_CERT_NUM**
[`string`](../../../data-types.md) | Number of State Registration Certificate ||
|| **RQ_ST_CERT_DATE**
[`string`](../../../data-types.md) | Date of State Registration Certificate ||
|| **RQ_VAT_PAYER**
[`char`](../../../data-types.md) | VAT payer (for country UA).

Values `Y` or `N` are used ||
|| **RQ_VAT_ID**
[`string`](../../../data-types.md) | VAT ID (identification number of VAT payer) ||
|| **RQ_VAT_CERT_SER**
[`string`](../../../data-types.md) | Series of the VAT certificate ||
|| **RQ_VAT_CERT_NUM**
[`string`](../../../data-types.md) | Number of the VAT certificate ||
|| **RQ_VAT_CERT_DATE**
[`string`](../../../data-types.md) | Date of the VAT certificate ||
|| **RQ_RESIDENCE_COUNTRY**
[`string`](../../../data-types.md) | Country of residence ||
|| **RQ_BASE_DOC**
[`string`](../../../data-types.md) | Basis for action ||
|| **RQ_REGON**
[`string`](../../../data-types.md) | REGON (for country PL) ||
|| **RQ_KRS**
[`string`](../../../data-types.md) | KRS (for country PL) ||
|| **RQ_PESEL**
[`string`](../../../data-types.md) | PESEL (for country PL) ||
|| **RQ_LEGAL_FORM**
[`string`](../../../data-types.md) | Legal form (for country FR) ||
|| **RQ_SIRET**
[`string`](../../../data-types.md) | Siret number (for country FR) ||
|| **RQ_SIREN**
[`string`](../../../data-types.md) | Siren number (for country FR) ||
|| **RQ_CAPITAL**
[`string`](../../../data-types.md) | Share capital (for country FR) ||
|| **RQ_RCS**
[`string`](../../../data-types.md) | RCS (for country FR) ||
|| **RQ_CNPJ**
[`string`](../../../data-types.md) | CNPJ (for country BR) ||
|| **RQ_STATE_REG**
[`string`](../../../data-types.md) | State Registration (IE) (for country BR) ||
|| **RQ_MNPL_REG**
[`string`](../../../data-types.md) | Municipal Registration (IM) (for country BR) ||
|| **RQ_CPF**
[`string`](../../../data-types.md) | CPF (for country BR) ||
|| **UF_CRM_...** | Custom fields. For example, `UF_CRM_1694526604`.

Requisites can have a set of custom fields with types: `string`, `boolean`, `double`, `datetime`.

You can add a custom field to requisites using the method [crm.requisite.userfield.add](../user-fields/crm-requisite-userfield-add.md) ||
|#

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method — access permissions are checked against the contact or company that owns the requisite

#|
|| **Method** | **Description** ||
|| [crm.requisite.add](./crm-requisite-add.md) | Creates a new requisite ||
|| [crm.requisite.update](./crm-requisite-update.md) | Updates an existing requisite ||
|| [crm.requisite.get](./crm-requisite-get.md) | Returns the requisite by identifier ||
|| [crm.requisite.list](./crm-requisite-list.md) | Returns a list of requisites by filter ||
|| [crm.requisite.delete](./crm-requisite-delete.md) | Deletes the requisite and all related objects ||
|| [crm.requisite.fields](./crm-requisite-fields.md) | Returns the description of requisite fields ||
|#
