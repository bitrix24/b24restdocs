# CRM Banking Details: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bank details are a strict sequence of numbers required for conducting operations with a bank account. They can be used to send a cashless payment or deposit money into the account through the cashier.

The mandatory bank details include:
- account number
- correspondent account number
- bank identification code (BIC) and full name of the bank

In CRM, bank details are linked to the universal details object. Multiple bank details can be associated with a single detail.

> Quick navigation: [All Methods](#all-methods)

## Getting Started

1. Retrieve the details identifier `ENTITY_ID` using the [crm.requisite.list](../universal/crm-requisite-list.md) method
2. Create a bank detail using the [crm.requisite.bankdetail.add](./crm-requisite-bank-detail-add.md) method
3. Update account data using the [crm.requisite.bankdetail.update](./crm-requisite-bank-detail-update.md) method
4. Retrieve a bank detail by ID using the [crm.requisite.bankdetail.get](./crm-requisite-bank-detail-get.md) method
5. Find Company details by filter using the [crm.requisite.bankdetail.list](./crm-requisite-bank-detail-list.md) method
6. Delete a bank detail using the [crm.requisite.bankdetail.delete](./crm-requisite-bank-detail-delete.md) method

## Bank Detail Identifiers

- `ID` — the bank detail identifier. It is returned by the [crm.requisite.bankdetail.add](./crm-requisite-bank-detail-add.md), [crm.requisite.bankdetail.get](./crm-requisite-bank-detail-get.md), and [crm.requisite.bankdetail.list](./crm-requisite-bank-detail-list.md) methods
- `ENTITY_ID` — the identifier of the details to which the bank detail is linked. It can be retrieved using the [crm.requisite.list](../universal/crm-requisite-list.md) method
- `COUNTRY_ID` — the country identifier for the set of bank fields. Available values are returned by the [crm.requisite.preset.countries](../presets/crm-requisite-preset-countries.md) method

## Bank Detail Fields

Mandatory fields are marked with `*`.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the bank details. Automatically created and unique within the account ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Parent object identifier. Currently, it can only be an attribute identifier.

Attribute identifiers can be obtained using the [`crm.requisite.list`](../universal/crm-requisite-list.md) method ||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of bank details fields (see method [crm.requisite.preset.countries](../presets/crm-requisite-preset-countries.md) for available values).

The country code of the bank details matches the country code in the linked requisite template, the identifier of which is specified in the `ENTITY_ID` field
||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Create date ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the requisite ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the bank details ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | Foreign key. Used for exchange operations. Identifier of an object in an external information base.

The purpose of the field may be changed by the end developer. Each application ensures the uniqueness of values in this field.

It is recommended to use a unique prefix to avoid collisions with other applications ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity flag. Uses values `Y` or `N`.

Currently, the field does not actually affect anything ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **RQ_BANK_NAME**
[`string`](../../../data-types.md) | Name of the bank ||
|| **RQ_BANK_ADDR**
[`string`](../../../data-types.md) | Address of the bank ||
|| **RQ_BANK_CODE**
[`string`](../../../data-types.md) | Bank Code (for country BR) ||
|| **RQ_BANK_ROUTE_NUM**
[`string`](../../../data-types.md) | Bank Routing Number ||
|| **RQ_BIK**
[`string`](../../../data-types.md) | BIC ||
|| **RQ_CODEB**
[`string`](../../../data-types.md) | Code Banque (for country FR) ||
|| **RQ_CODEG**
[`string`](../../../data-types.md) | Code Guichet (for country FR) ||
|| **RQ_RIB**
[`string`](../../../data-types.md) | Clé RIB (for country FR) ||
|| **RQ_MFO**
[`string`](../../../data-types.md) | MFO ||
|| **RQ_ACC_NAME**
[`string`](../../../data-types.md) | Bank Account Holder Name ||
|| **RQ_ACC_NUM**
[`string`](../../../data-types.md) | Bank Account Number ||
|| **RQ_ACC_TYPE**
[`string`](../../../data-types.md) | Tipo da conta (for country BR) ||
|| **RQ_AGENCY_NAME**
[`string`](../../../data-types.md) | Agência (for country BR) ||
|| **RQ_IIK**
[`string`](../../../data-types.md) | IIK ||
|| **RQ_ACC_CURRENCY**
[`string`](../../../data-types.md) | Currency of the account ||
|| **RQ_COR_ACC_NUM**
[`string`](../../../data-types.md) | Correspondent account ||
|| **RQ_IBAN**
[`string`](../../../data-types.md) | IBAN ||
|| **RQ_SWIFT**
[`string`](../../../data-types.md) | SWIFT ||
|| **RQ_BIC**
[`string`](../../../data-types.md) | BIC ||
|| **COMMENTS**
[`string`](../../../data-types.md) | Comment ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the external information base. The purpose of the field may change by the final developer ||
|#

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method — access permissions are checked against the contact or company that owns the requisite

#|
|| **Method** | **Description** ||
|| [crm.requisite.bankdetail.add](./crm-requisite-bank-detail-add.md) | Creates a new bank detail ||
|| [crm.requisite.bankdetail.update](./crm-requisite-bank-detail-update.md) | Modifies an existing bank detail ||
|| [crm.requisite.bankdetail.get](./crm-requisite-bank-detail-get.md) | Returns the bank detail by identifier ||
|| [crm.requisite.bankdetail.list](./crm-requisite-bank-detail-list.md) | Returns a list of bank details by filter ||
|| [crm.requisite.bankdetail.delete](./crm-requisite-bank-detail-delete.md) | Deletes a bank detail ||
|| [crm.requisite.bankdetail.fields](./crm-requisite-bank-detail-fields.md) | Returns a formal description of bank detail fields ||
|#
