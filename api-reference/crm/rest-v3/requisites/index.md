# Requisites in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Requisites store the legal details of a contact or company: the organization name, TIN, KPP, OGRN, and personal ID document data. A requisite includes bank details, which are bank accounts, and addresses. Client and my company requisites are selected in a deal, estimate, invoice, or SPA item to be inserted into documents.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [add company details](https://helpdesk.bitrix24.com/open/5870771/)

## How to Choose a Method

#|
|| **Task** | **Methods** ||
|| Find out which requisite templates exist and which fields they allow | [crm.&lt;type&gt;.requisite.preset.list](./crm-entity-requisite-preset-list-rest-v3.md), [crm.&lt;type&gt;.requisite.preset.get](./crm-entity-requisite-preset-get-rest-v3.md) ||
|| Create, update, retrieve, or delete a contact or company requisite | [crm.&lt;type&gt;.requisite.add](./crm-entity-requisite-add-rest-v3.md), [update](./crm-entity-requisite-update-rest-v3.md), [get](./crm-entity-requisite-get-rest-v3.md), [list](./crm-entity-requisite-list-rest-v3.md), [delete](./crm-entity-requisite-delete-rest-v3.md) ||
|| Add a bank account to a requisite | [crm.&lt;type&gt;.requisite.bankrequisite.*](./crm-entity-requisite-bank-requisite-add-rest-v3.md) ||
|| Retrieve all bank accounts of a contact or company | [crm.&lt;type&gt;.bankrequisite.list](./crm-entity-bank-requisite-list-rest-v3.md) ||
|| Add a legal, actual, or another address to a requisite | [crm.&lt;type&gt;.requisite.address.*](./crm-entity-requisite-address-add-rest-v3.md) ||
|| Select client requisites in a deal, estimate, invoice, CRM document, or SPA item | [crm.&lt;type&gt;.clientrequisite.get](./crm-entity-client-requisite-get-rest-v3.md), [crm.&lt;type&gt;.clientrequisite.set](./crm-entity-client-requisite-set-rest-v3.md) ||
|| Select my company requisites in a deal or invoice | [crm.&lt;type&gt;.mycompanyrequisite.get](./crm-entity-my-company-requisite-get-rest-v3.md), [crm.&lt;type&gt;.mycompanyrequisite.set](./crm-entity-my-company-requisite-set-rest-v3.md) ||
|| Export or search the requisites of all contacts and companies | [crm.requisite.list](./crm-requisite-list-rest-v3.md) ||
|#

Method names are written in lowercase: `crm.company.requisite.bankrequisite.add`.

## How to Get Started

1. Select a template with the [crm.company.requisite.preset.list](./crm-entity-requisite-preset-list-rest-v3.md) method. The template defines which fields can be filled in: the `fields` elements with `kind: "requisiteField"` are the requisite field names
2. Create a requisite with the [crm.company.requisite.add](./crm-entity-requisite-add-rest-v3.md) method: the `companyId` owner, the `presetId` template, the `name`, and the template fields
3. Add a bank account with the [crm.company.requisite.bankrequisite.add](./crm-entity-requisite-bank-requisite-add-rest-v3.md) method and an address with the [crm.company.requisite.address.add](./crm-entity-requisite-address-add-rest-v3.md) method
4. Select the requisite and the bank account in a deal with the [crm.deal.clientrequisite.set](./crm-entity-client-requisite-set-rest-v3.md) method

## Relations to Other Objects

Requisites are related to their owner, their template, and the CRM items in which they are selected.

**Contact and company.** A requisite belongs to one contact or one company. The owner type is set by the `contact` or `company` segment of the method name, and the ID by the `contactId` or `companyId` field. The owner cannot be changed after creation. The segment in the method name must match the owner: the `crm.contact.requisite.get` method does not find a company requisite. Permission to view the owner grants permission to read its requisites, and permission to edit grants permission to change them.

**My company.** My company requisites are read and changed with the `crm.company.requisite.*` and `crm.company.bankrequisite.list` methods using the my company ID. My company IDs are returned by the [crm.mycompany.list](../items/crm-entity-list-rest-v3.md) method.

**Template.** The `presetId` field is the requisite template: organization, sole proprietor, person, and templates of other countries. Only the fields of its template can be written to a requisite. The set of bank details fields also depends on the template country. Templates are configured in the Bitrix24 interface; the methods of the group only read them.

**Deal, estimate, invoice, CRM document, SPA.** The client requisite and bank details are selected in an item with the `crm.<type>.clientrequisite.*` methods. In deals and invoices, my company requisite is also selected, with the `crm.<type>.mycompanyrequisite.*` methods. These requisites are inserted into the item documents.

**Addresses.** A requisite address is identified by the `requisiteId` and `typeId` pair. Contact or company addresses not tied to a specific requisite are handled by the methods of the [Addresses](../addresses/index.md) group.

## Object Types

#|
|| **Methods** | **Object types** ||
|| `crm.<type>.requisite.*`, `crm.<type>.bankrequisite.list` | Company `company`, contact `contact` ||
|| `crm.<type>.clientrequisite.*` | Deal `deal`, estimate `quote`, invoice `smartinvoice`, CRM document `smartdocument`, SPA `smartprocess<entityTypeId>` ||
|| `crm.<type>.mycompanyrequisite.*` | Deal `deal`, invoice `smartinvoice` ||
|| `crm.requisite.list` | No type, requisites of all contacts and companies ||
|#

SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

## Requisite Fields {#fields}

Field names are written in camelCase. Fields are returned in the response only if they are listed in `select`. Empty fields are returned as `null`. Only the fields of its template can be written to a requisite.

#|
|| **Field**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Requisite ID. Read-only ||
|| **companyId**, **contactId**
[`integer`](../../../data-types.md) | Requisite owner. Set at creation and does not change. In `crm.requisite.list`, `ownerTypeId` and `ownerId` are used instead ||
|| **presetId**
[`integer`](../../../data-types.md) | Requisite template ID ||
|| **name**
[`string`](../../../data-types.md) | Requisite name ||
|| **active**
[`boolean`](../../../data-types.md) | `true` if the requisite is active, `false` if inactive ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order ||
|| **xmlId**
[`string`](../../../data-types.md) | External code ||
|| **originatorId**
[`string`](../../../data-types.md) | ID of the external data source ||
|| **createdTime**, **updatedTime**
[`datetime`](../../../data-types.md) | Creation and modification date. Read-only ||
|| **createdById**, **updatedById**
[`integer`](../../../data-types.md) | Who created and modified the requisite. Read-only ||
|| **fullName**, **firstName**, **lastName**, **secondName**
[`string`](../../../data-types.md) | Full name, first name, last name, second name ||
|| **companyTitle**, **companyFullName**
[`string`](../../../data-types.md) | Short and full organization name ||
|| **companyIdentifier**
[`string`](../../../data-types.md) | Organization identifier ||
|| **companyRegistrationDate**
[`string`](../../../data-types.md) | State registration date ||
|| **directorName**, **accountantName**
[`string`](../../../data-types.md) | General director, chief accountant ||
|| **ceoName**, **ceoPosition**
[`string`](../../../data-types.md) | Name and position of the head ||
|| **contactPerson**, **emailAddress**, **phoneNumber**, **faxNumber**
[`string`](../../../data-types.md) | Contact person, email, phone, fax ||
|| **identificationTypeId**
[`string`](../../../data-types.md) | Identification method ||
|| **identityDocumentType**, **identityDocumentSeries**, **identityDocumentNumber**, **identityDocumentPersonalNumber**, **identityDocumentIssueDate**, **identityDocumentIssuedBy**, **identityDocumentDepartmentCode**
[`string`](../../../data-types.md) | Identity document: type, series, number, personal number, issue date, issuing authority, department code ||
|| **inn**, **kpp**, **ogrn**, **ogrnip**, **okpo**, **oktmo**, **okved**, **ifns**
[`string`](../../../data-types.md) | TIN, KPP, OGRN, OGRNIP, OKPO, OKTMO, OKVED, IFNS ||
|| **basisDocument**
[`string`](../../../data-types.md) | Basis of authority ||
|| **registrationCertificateSeries**, **registrationCertificateNumber**, **registrationCertificateDate**
[`string`](../../../data-types.md) | Series, number, and date of the state registration certificate ||
|| **vatPayer**
[`boolean`](../../../data-types.md) | `true` if a VAT payer, `false` otherwise ||
|| **vatNumber**, **vatCertificateSeries**, **vatCertificateNumber**, **vatCertificateDate**
[`string`](../../../data-types.md) | VAT ID, series, number, and date of the VAT payer certificate ||
|| **residenceCountry**
[`string`](../../../data-types.md) | Country of residence ||
|| **edrpou**, **drfo**
[`string`](../../../data-types.md) | EDRPOU and DRFO, for Ukraine ||
|| **kbe**, **iin**, **bin**
[`string`](../../../data-types.md) | KBE, IIN, BIN, for Kazakhstan ||
|| **usrle**
[`string`](../../../data-types.md) | Handelsregisternummer, for Germany ||
|| **regon**, **krs**, **pesel**
[`string`](../../../data-types.md) | REGON, KRS, PESEL, for Poland ||
|| **siret**, **siren**, **rcs**, **legalForm**, **shareCapital**
[`string`](../../../data-types.md) | SIRET, SIREN, RCS, legal form, share capital, for France ||
|| **cnpj**, **cpf**, **stateRegistration**, **municipalRegistration**, **taxRegimeId**
[`string`](../../../data-types.md) | CNPJ, CPF, state and municipal registration, tax regime, for Brazil ||
|| **addresses**
[`object[]`](../../../data-types.md) | Requisite addresses. Read-only; returned by the `crm.<type>.requisite.get` and `list` methods if requested in `select`. [Address fields](#address-fields) ||
|| **bankRequisites**
[`object[]`](../../../data-types.md) | Bank details. Read-only; returned in the same way as `addresses`. [Fields](#nested-bank-fields) ||
|#

Value length is limited. If a value is longer, the method returns an error with the allowed length, for example, "The field value must not exceed 15 characters" for `inn`.

## Bank Details Fields {#bank-fields}

The set of bank fields depends on the country of the requisite template. For example, for the German template, it is `name`, `comments`, `bankName`, `bankAddress`, `bankRoutingNumber`, `bankAccountHolderName`, `bankAccountNumber`, `iban`, `swift`, `bic`. Read methods return all fields of the set, with empty ones as `null`. Write methods reject a field outside the set.

#|
|| **Field**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Bank details ID. Read-only ||
|| **requisiteId**
[`integer`](../../../data-types.md) | Requisite ID. Set at creation and does not change ||
|| **name**
[`string`](../../../data-types.md) | Name, up to 255 characters ||
|| **comments**
[`string`](../../../data-types.md) | Comment ||
|| **bankName**, **bankAddress**
[`string`](../../../data-types.md) | Bank name and address, up to 255 characters ||
|| **bik**
[`string`](../../../data-types.md) | BIK, up to 11 characters ||
|| **bankAccountNumber**
[`string`](../../../data-types.md) | Bank account number, up to 34 characters ||
|| **correspondentAccountNumber**
[`string`](../../../data-types.md) | Correspondent account, up to 34 characters ||
|| **bankAccountCurrency**
[`string`](../../../data-types.md) | Account currency, up to 100 characters ||
|| **bankAccountHolderName**
[`string`](../../../data-types.md) | Account holder, up to 150 characters ||
|| **bankAccountType**
[`string`](../../../data-types.md) | Account type, up to 50 characters ||
|| **bankCode**, **bankAgencyName**
[`string`](../../../data-types.md) | Bank code and branch name, up to 50 characters ||
|| **bankRoutingNumber**
[`string`](../../../data-types.md) | Routing Number, up to 9 characters ||
|| **iban**
[`string`](../../../data-types.md) | IBAN, up to 34 characters ||
|| **swift**, **bic**
[`string`](../../../data-types.md) | SWIFT and BIC, up to 11 characters ||
|| **mfo**
[`string`](../../../data-types.md) | MFO, up to 6 characters, for Ukraine ||
|| **iik**
[`string`](../../../data-types.md) | IIK, up to 20 characters, for Kazakhstan ||
|| **codeb**, **codeg**, **rib**
[`string`](../../../data-types.md) | Code Banque and Code Guichet, up to 5 characters, Clé RIB, up to 2 characters, for France ||
|#

### Bank Details Inside a Requisite {#nested-bank-fields}

In `bankRequisites` of the `crm.<type>.requisite.get` and `list` methods, the fields are named differently than in the bank details methods, and empty values are returned as empty strings. The fields are `id`, `name`, `active`, `sort`, `comments`, `bankName`, `bankAddress`, `bankCode`, `bankRoutingNumber`, `agencyName`, `bik`, `mfo`, `accountName`, `accountNumber`, `accountType`, `accountCurrency`, `correspondentAccountNumber`, `iik`, `iban`, `swift`, `bic`, `codeB`, `codeG`, `rib`.

## Requisite Address Fields {#address-fields}

An address is identified by the `requisiteId` and `typeId` pair. Empty fields are returned as empty strings.

#|
|| **Field**
`type` | **Description** ||
|| **requisiteId**
[`integer`](../../../data-types.md) | Requisite ID. Set at creation ||
|| **typeId**
[`integer`](../../../data-types.md) | Address type: `1` for the actual address, `4` for the registration address, `6` for the legal address, `9` for the beneficiary address, `11` for the delivery address. The list is returned by the [crm.dictionary.addresstype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method. Set at creation ||
|| **address1**, **address2**
[`string`](../../../data-types.md) | Street and building, apartment or office ||
|| **city**, **postalCode**
[`string`](../../../data-types.md) | City, postal code ||
|| **region**, **province**
[`string`](../../../data-types.md) | District, region ||
|| **country**
[`string`](../../../data-types.md) | Country ||
|| **countryCode**
[`string`](../../../data-types.md) | Country code from the Bitrix24 location directory. Retained only if it matches the country name in `country` ||
|#

## Field Methods

Each group has field description methods: `crm.<type>.requisite.field.list` and `field.get`, `crm.<type>.requisite.preset.field.list` and `field.get`, `crm.<type>.requisite.bankrequisite.field.list` and `field.get`, `crm.<type>.requisite.address.field.list` and `field.get`, `crm.<type>.clientrequisite.field.list` and `field.get`, `crm.<type>.mycompanyrequisite.field.list` and `field.get`, `crm.requisite.field.list` and `field.get`. They work like the other REST 3.0 field methods: `field.list` accepts `select` with a list of attributes, for example, `["name","type","editable","filterable","sortable"]`, and `field.get` also accepts `name` with the field name.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Requisites

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.requisite.add](./crm-entity-requisite-add-rest-v3.md) | Adds a requisite to a contact or company ||
|| [crm.&lt;type&gt;.requisite.update](./crm-entity-requisite-update-rest-v3.md) | Updates a requisite ||
|| [crm.&lt;type&gt;.requisite.get](./crm-entity-requisite-get-rest-v3.md) | Returns a requisite by ID ||
|| [crm.&lt;type&gt;.requisite.list](./crm-entity-requisite-list-rest-v3.md) | Returns the requisites of a contact or company ||
|| [crm.&lt;type&gt;.requisite.delete](./crm-entity-requisite-delete-rest-v3.md) | Deletes a requisite ||
|| [crm.requisite.list](./crm-requisite-list-rest-v3.md) | Returns the requisites of all contacts and companies ||
|#

### Requisite Templates

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.requisite.preset.get](./crm-entity-requisite-preset-get-rest-v3.md) | Returns a requisite template with its fields ||
|| [crm.&lt;type&gt;.requisite.preset.list](./crm-entity-requisite-preset-list-rest-v3.md) | Returns all active templates ||
|#

### Bank Details

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.requisite.bankrequisite.add](./crm-entity-requisite-bank-requisite-add-rest-v3.md) | Adds bank details to a requisite ||
|| [crm.&lt;type&gt;.requisite.bankrequisite.update](./crm-entity-requisite-bank-requisite-update-rest-v3.md) | Updates bank details ||
|| [crm.&lt;type&gt;.requisite.bankrequisite.get](./crm-entity-requisite-bank-requisite-get-rest-v3.md) | Returns bank details by ID ||
|| [crm.&lt;type&gt;.requisite.bankrequisite.list](./crm-entity-requisite-bank-requisite-list-rest-v3.md) | Returns the bank details of a requisite ||
|| [crm.&lt;type&gt;.requisite.bankrequisite.delete](./crm-entity-requisite-bank-requisite-delete-rest-v3.md) | Deletes bank details ||
|| [crm.&lt;type&gt;.bankrequisite.list](./crm-entity-bank-requisite-list-rest-v3.md) | Returns the bank details of a contact or company ||
|#

### Requisite Addresses

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.requisite.address.add](./crm-entity-requisite-address-add-rest-v3.md) | Adds an address to a requisite ||
|| [crm.&lt;type&gt;.requisite.address.update](./crm-entity-requisite-address-update-rest-v3.md) | Updates a requisite address ||
|| [crm.&lt;type&gt;.requisite.address.get](./crm-entity-requisite-address-get-rest-v3.md) | Returns a requisite address by type ||
|| [crm.&lt;type&gt;.requisite.address.list](./crm-entity-requisite-address-list-rest-v3.md) | Returns the addresses of a requisite ||
|| [crm.&lt;type&gt;.requisite.address.delete](./crm-entity-requisite-address-delete-rest-v3.md) | Deletes a requisite address ||
|#

### Requisites in CRM Items

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.clientrequisite.get](./crm-entity-client-requisite-get-rest-v3.md) | Returns the client requisites selected in an item ||
|| [crm.&lt;type&gt;.clientrequisite.set](./crm-entity-client-requisite-set-rest-v3.md) | Selects the client requisites in an item ||
|| [crm.&lt;type&gt;.mycompanyrequisite.get](./crm-entity-my-company-requisite-get-rest-v3.md) | Returns my company requisites selected in a deal or invoice ||
|| [crm.&lt;type&gt;.mycompanyrequisite.set](./crm-entity-my-company-requisite-set-rest-v3.md) | Selects my company requisites in a deal or invoice ||
|#
