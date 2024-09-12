# About Bank Details

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform methods: any user

Bank details are a strict sequence of numbers required for conducting operations with a checking account. With these details, you can send a cashless payment or deposit money into the account through the cashier.

The mandatory bank details include:
- account number
- correspondent account number
- bank identification code (BIC) and full name of the bank

In CRM, bank details are linked to the universal details object. Multiple bank details can be associated with a single detail.

# Fields of Bank Details

#|
|| **Name**
`type` | **Description** | **Read** | **Write** | **Required** | **Immutable** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the bank detail. Created automatically and unique within the account | Yes | No | No | No ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object's type. Can only be `Detail` (value `8`).

Object type identifiers are returned by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) | Yes | No | No | No ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object | Yes | Yes | Yes | Yes ||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of bank detail fields (see the method [crm.requisite.preset.countries](../presets/crm-requisite-preset-countries.md) for available values).

The country code of the bank detail matches the country code in the linked detail template, the identifier of which is specified in the `ENTITY_ID` field | Yes | Yes | No | No ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Creation date | Yes | No | No | No ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date | Yes | No | No | No ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the detail | Yes | No | No | No ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the detail | Yes | No | No | No ||
|| **NAME^*^**
[`string`](../../../data-types.md) | Name of the bank detail | Yes | Yes | No | No ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the detail | Yes | Yes | No | No ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information base.

The purpose of the field may change by the final developer. Each application ensures the uniqueness of values in this field.

It is recommended to use a unique prefix to avoid collisions with other applications | Yes | Yes | No | No ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity indicator. Uses values `Y` or `N`.

Currently, the field does not actually affect anything | Yes | Yes | No | No ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting | Yes | Yes | No | No ||
|| **RQ_BANK_NAME**
[`string`](../../../data-types.md) | Name of the bank | Yes | Yes | No | No ||
|| **RQ_BANK_ADDR**
[`string`](../../../data-types.md) | Address of the bank | Yes | Yes | No | No ||
|| **RQ_BANK_CODE**
[`string`](../../../data-types.md) | Bank Code (for country BR) | Yes | Yes | No | No ||
|| **RQ_BANK_ROUTE_NUM**
[`string`](../../../data-types.md) | Bank Routing Number | Yes | Yes | No | No ||
|| **RQ_BIK**
[`string`](../../../data-types.md) | BIC | Yes | Yes | No | No ||
|| **RQ_CODEB**
[`string`](../../../data-types.md) | Code Banque (for country FR) | Yes | Yes | No | No ||
|| **RQ_CODEG**
[`string`](../../../data-types.md) | Code Guichet (for country FR) | Yes | Yes | No | No ||
|| **RQ_RIB**
[`string`](../../../data-types.md) | Clé RIB (for country FR) | Yes | Yes | No | No ||
|| **RQ_MFO**
[`string`](../../../data-types.md) | MFO | Yes | Yes | No | No ||
|| **RQ_ACC_NAME**
[`string`](../../../data-types.md) | Bank Account Holder Name | Yes | Yes | No | No ||
|| **RQ_ACC_NUM**
[`string`](../../../data-types.md) | Bank Account Number | Yes | Yes | No | No ||
|| **RQ_ACC_TYPE**
[`string`](../../../data-types.md) | Tipo da conta (for country BR) | Yes | Yes | No | No ||
|| **RQ_AGENCY_NAME**
[`string`](../../../data-types.md) | Agência (for country BR) | Yes | Yes | No | No ||
|| **RQ_IIK**
[`string`](../../../data-types.md) | IIK | Yes | Yes | No | No ||
|| **RQ_ACC_CURRENCY**
[`string`](../../../data-types.md) | Currency of the account | Yes | Yes | No | No ||
|| **RQ_COR_ACC_NUM**
[`string`](../../../data-types.md) | Correspondent account | Yes | Yes | No | No ||
|| **RQ_IBAN**
[`string`](../../../data-types.md) | IBAN | Yes | Yes | No | No ||
|| **RQ_SWIFT**
[`string`](../../../data-types.md) | SWIFT | Yes | Yes | No | No ||
|| **RQ_BIC**
[`string`](../../../data-types.md) | BIC | Yes | Yes | No | No ||
|| **COMMENTS**
[`string`](../../../data-types.md) | Comment | Yes | Yes | No | No ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the external information base. The purpose of the field may change by the final developer | Yes | Yes | No | No ||
|#

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [crm.requisite.bankdetail.add](./crm-requisite-bank-detail-add.md) | Creates a new bank detail ||
|| [crm.requisite.bankdetail.update](./crm-requisite-bank-detail-update.md) | Modifies an existing bank detail ||
|| [crm.requisite.bankdetail.get](./crm-requisite-bank-detail-get.md) | Returns a bank detail by identifier ||
|| [crm.requisite.bankdetail.list](./crm-requisite-bank-detail-list.md) | Returns a list of bank details by filter ||
|| [crm.requisite.bankdetail.delete](./crm-requisite-bank-detail-delete.md) | Deletes a bank detail ||
|| [crm.requisite.bankdetail.fields](./crm-requisite-bank-detail-fields.md) | Returns a formal description of bank detail fields ||
|#