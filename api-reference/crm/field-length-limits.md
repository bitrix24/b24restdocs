# CRM Field Length Limits

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Length limits are important when creating and updating CRM objects. If a value is longer than allowed, the method can return a validation error or retain a value truncated to the limit. The behavior depends on the specific field and the validation level: validator, ORM, or database constraint.

The tables show the maximum length of standard fields for main CRM objects: leads, deals, contacts, companies, estimates, invoices, Smart Processes, activities, addresses, requisites, bank details, product items, UTM fields, and multiple fields. Maximum length is measured in characters unless another unit is specified.

## How to Read the Tables

Tables and field settings can contain different types of limits:

- for string fields, the value in the table means the maximum number of characters
- for fields of type `file`, user text length does not apply: the field receives a file or a file identifier
- for fields of type `text`, `mediumText`, and `longText`, a separate character limit is not returned. The actual volume is limited by the storage type and general request limits
- for fields of type `integer`, `double`, `date`, `datetime`, `crm_status`, `crm`, `boolean`, the limit is checked as value correctness, not as string length: a number, date, object identifier, status code, or flag
- for custom fields, the limit depends on the field type and the settings of the specific field

To find the required limit:

- for custom fields, see [How to Get CRM Custom Field Limits](#user-field-limits)
- for lead, deal, contact, company, estimate, invoice, and Smart Process fields, see [Universal CRM Method Field Limits](#universal-fields)
- for UTM fields, multiple fields, product items, activities, addresses, requisites, and bank details, see [Related CRM Data Limits](#related-data)

## How to Get CRM Custom Field Limits {#user-field-limits}

Custom fields do not have a single common limit. The limit depends on the field type and its settings.

#|
|| **Custom Field Type** | **Where to Find the Limit** ||
|| `string`, `url`, `string_formatted` | The `SETTINGS.MAX_LENGTH` value in the field description. If `MAX_LENGTH = 0`, no separate length limit is set ||
|| `integer`, `double` | The `SETTINGS.MIN_VALUE` and `SETTINGS.MAX_VALUE` values. This is a value limit, not a character count limit ||
|| `file` | The `SETTINGS.MAX_ALLOWED_SIZE` value. This is the file size limit in bytes ||
|| `enumeration`, `boolean`, `date`, `datetime`, `crm`, `crm_status` | The limit is checked as value correctness, not as string length: list option, flag, date, object identifier, or status code ||
|#

You can retrieve custom field settings using these methods:

- [userfieldconfig.list](./universal/userfieldconfig/userfieldconfig-list.md) — for custom fields of universal CRM objects
- `crm.*.userfield.list` — for custom fields of specific CRM objects, for example [crm.lead.userfield.list](./leads/userfield/crm-lead-userfield-list.md), [crm.deal.userfield.list](./deals/user-defined-fields/crm-deal-userfield-list.md), [crm.contact.userfield.list](./contacts/userfield/crm-contact-userfield-list.md), [crm.company.userfield.list](./companies/userfields/crm-company-userfield-list.md)
- [crm.userfield.settings.fields](./universal/user-defined-fields/crm-userfield-settings-fields.md) — to find out which settings are supported by the custom field type

## Universal CRM Method Field Limits {#universal-fields}

The [crm.item.*](./universal/index.md) methods manage CRM objects: leads, deals, contacts, companies, invoices, estimates, and Smart Process items. To select the object type, pass the `entityTypeId` identifier.

### Lead

Lead fields are returned by the [crm.item.fields](./universal/crm-item-fields.md) method with `entityTypeId = 1`.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `title`, `companyTitle`, `post`, `originatorId`, `originId` | 255 ||
|| `honorific` | 128 ||
|| `stageId`, `sourceId`, `name`, `lastName`, `secondName`, `currencyId` | 50 ||
|| `stageSemanticId` | 3 ||
|| `opened`, `hasPhone`, `hasEmail`, `hasImol`, `isReturnCustomer`, `isManualOpportunity` | 1 ||
|#

### Deal

Deal fields are returned by the [crm.item.fields](./universal/crm-item-fields.md) method with `entityTypeId = 2`.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `title`, `originatorId`, `originId` | 255 ||
|| `locationId` | 100 ||
|| `stageId`, `previousStageId`, `typeId`, `currencyId`, `sourceId` | 50 ||
|| `stageSemanticId` | 3 ||
|| `opened`, `isNew`, `isRecurring`, `isReturnCustomer`, `isRepeatedApproach`, `closed`, `isManualOpportunity` | 1 ||
|#

### Contact

Contact fields are returned by the [crm.item.fields](./universal/crm-item-fields.md) method with `entityTypeId = 3`.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `post`, `originatorId`, `originId`, `originVersion` | 255 ||
|| `honorific` | 128 ||
|| `sourceId`, `name`, `lastName`, `secondName`, `typeId` | 50 ||
|| `photo` | 10 ||
|| `opened`, `export`, `hasPhone`, `hasEmail`, `hasImol` | 1 ||
|#

The `photo` field refers to files: the value `10` describes the length of the file identifier in storage, not the number of characters in user text.

### Company

Company fields are returned by the [crm.item.fields](./universal/crm-item-fields.md) method with `entityTypeId = 4`.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `title`, `originatorId`, `originId`, `originVersion` | 255 ||
|| `typeId`, `industry`, `currencyId`, `employees` | 50 ||
|| `logo` | 10 ||
|| `opened`, `hasPhone`, `hasEmail`, `hasImol`, `isMyCompany` | 1 ||
|#

The `logo` field refers to files: the value `10` describes the length of the file identifier in storage, not the number of characters in user text.

### Estimate

Estimate fields are returned by the [crm.item.fields](./universal/crm-item-fields.md) method with `entityTypeId = 7`.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `title` | 255 ||
|| `quoteNumber`, `locationId` | 100 ||
|| `stageId`, `currencyId` | 50 ||
|| `opened`, `closed`, `isManualOpportunity` | 1 ||
|#

### Invoices

Invoice fields are returned by the [crm.item.fields](./universal/crm-item-fields.md) method with `entityTypeId = 31`.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `title` | 255 ||
|| `accountNumber`, `locationId` | 100 ||
|| `stageId`, `previousStageId`, `currencyId`, `sourceId` | 50 ||
|| `opened`, `isManualOpportunity`, `isRecurring` | 1 ||
|#

The `comments` field has the `text` type, so a separate character limit is not returned for it.

### Smart Processes

Smart Process fields are returned by the [crm.item.fields](./universal/crm-item-fields.md) method with the Smart Process `entityTypeId`. The set of standard fields depends on the type settings.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `title` | 255 ||
|| `stageId`, `previousStageId`, `currencyId`, `sourceId` | 50 ||
|| `opened`, `isManualOpportunity` | 1 ||
|#

Check Smart Process fields created by users through the custom field settings. No separate length limit is set for the `xmlId` field.

## Related CRM Data Limits {#related-data}

### UTM Fields

The `utmSource`, `utmMedium`, `utmCampaign`, `utmContent`, and `utmTerm` UTM fields are available in leads, deals, contacts, companies, estimates, invoices, and Smart Processes.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| Value of each UTM field | 600 ||
|#

### Multiple Fields

Multiple fields for phones, email addresses, websites, messengers, and links are described by the [crm_multifield](./data-types.md#crm_multifield) type.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `VALUE` | 250 ||
|| `COMPLEX_ID` | 100 ||
|| `VALUE_TYPE` | 50 ||
|| `ENTITY_ID`, `TYPE_ID` | 16 ||
|#

### Product Items

Product items are used in the [crm.item.productrow.*](./universal/product-rows/index.md) methods.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `PRODUCT_NAME` | 256 ||
|| `XML_ID` | 255 ||
|| `MEASURE_NAME` | 50 ||
|| `OWNER_TYPE` | 20 ||
|| `TAX_INCLUDED`, `CUSTOMIZED` | 1 ||
|#

### Activities

Activity fields are returned by the [crm.activity.fields](./timeline/activities/activity-base/crm-activity-fields.md) method.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `SUBJECT` | 512 ||
|| `LOCATION` | 256 ||
|| `ORIGINATOR_ID`, `ORIGIN_ID`, `RESULT_SOURCE_ID` | 255 ||
|| `PROVIDER_ID`, `PROVIDER_TYPE_ID`, `PROVIDER_GROUP_ID` | 100 ||
|| `URN` | 64 ||
|| `RESULT_CURRENCY_ID` | 3 ||
|| `IS_HANDLEABLE`, `COMPLETED` | 1 ||
|#

### Addresses

Address fields are returned by the [crm.address.fields](./requisites/addresses/crm-address-fields.md) method.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `ADDRESS_1`, `ADDRESS_2` | 1024 ||
|| `CITY`, `REGION`, `PROVINCE`, `COUNTRY` | 128 ||
|| `COUNTRY_CODE` | 100 ||
|| `POSTAL_CODE` | 16 ||
|#

### Requisites

Requisite fields are returned by the [crm.requisite.fields](./requisites/universal/crm-requisite-fields.md) method.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `RQ_COMPANY_FULL_NAME` | 300 ||
|| `NAME`, `ORIGINATOR_ID`, `RQ_COMPANY_ID`, `RQ_COMPANY_NAME`, `RQ_EMAIL`, `RQ_IDENT_DOC`, `RQ_IDENT_DOC_ISSUED_BY`, `RQ_IFNS`, `RQ_OKVED`, `RQ_BASE_DOC` | 255 ||
|| `RQ_NAME`, `RQ_DIRECTOR`, `RQ_ACCOUNTANT`, `RQ_CEO_NAME`, `RQ_CEO_WORK_POS`, `RQ_CONTACT` | 150 ||
|| `RQ_RESIDENCE_COUNTRY` | 128 ||
|| `RQ_LEGAL_FORM` | 80 ||
|| `RQ_FIRST_NAME`, `RQ_LAST_NAME`, `RQ_SECOND_NAME`, `RQ_IDENT_TYPE`, `RQ_TAX_REGIME`, `RQ_RCS` | 50 ||
|| `CODE`, `XML_ID` | 45 ||
|| `RQ_COMPANY_REG_DATE`, `RQ_PHONE`, `RQ_FAX`, `RQ_IDENT_DOC_DATE`, `RQ_ST_CERT_DATE`, `RQ_VAT_CERT_DATE`, `RQ_CAPITAL` | 30 ||
|| `RQ_IDENT_DOC_SER`, `RQ_IDENT_DOC_NUM`, `RQ_IDENT_DOC_PERS_NUM`, `RQ_IDENT_DOC_DEP_CODE`, `RQ_STATE_REG` | 25 ||
|| `RQ_USRLE`, `RQ_VAT_ID`, `RQ_SIRET`, `RQ_CNPJ`, `RQ_MNPL_REG`, `RQ_CPF` | 20 ||
|| `RQ_INN`, `RQ_OGRNIP`, `RQ_ST_CERT_NUM`, `RQ_VAT_CERT_NUM`, `RQ_SIREN` | 15 ||
|| `RQ_OGRN` | 13 ||
|| `RQ_OKPO`, `RQ_IIN`, `RQ_BIN` | 12 ||
|| `RQ_OKTMO`, `RQ_PESEL` | 11 ||
|| `RQ_EDRPOU`, `RQ_DRFO`, `RQ_ST_CERT_SER`, `RQ_VAT_CERT_SER`, `RQ_KRS` | 10 ||
|| `RQ_KPP`, `RQ_REGON` | 9 ||
|| `RQ_KBE` | 2 ||
|| `ACTIVE`, `ADDRESS_ONLY`, `RQ_VAT_PAYER` | 1 ||
|#

### Bank Details

Bank detail fields are returned by the [crm.requisite.bankdetail.fields](./requisites/bank-detail/crm-requisite-bank-detail-fields.md) method.

#|
|| **Fields** | **Maximum Length, Characters** ||
|| `COMMENTS` | 500 ||
|| `NAME`, `ORIGINATOR_ID`, `RQ_BANK_NAME`, `RQ_BANK_ADDR` | 255 ||
|| `RQ_ACC_NAME` | 150 ||
|| `RQ_ACC_CURRENCY` | 100 ||
|| `RQ_BANK_CODE`, `RQ_ACC_TYPE`, `RQ_AGENCY_NAME` | 50 ||
|| `CODE`, `XML_ID` | 45 ||
|| `RQ_ACC_NUM`, `RQ_COR_ACC_NUM`, `RQ_IBAN` | 34 ||
|| `RQ_IIK` | 20 ||
|| `RQ_BIK`, `RQ_SWIFT`, `RQ_BIC` | 11 ||
|| `RQ_BANK_ROUTE_NUM` | 9 ||
|| `RQ_MFO` | 6 ||
|| `RQ_CODEB`, `RQ_CODEG` | 5 ||
|| `RQ_RIB` | 2 ||
|| `ACTIVE` | 1 ||
|#

## Continue Learning

- [Fields of Main CRM Objects](./main-entities-fields.md)
- [CRM Object Fields](./universal/object-fields.md)
- [Retrieve Item Fields crm.item.fields](./universal/crm-item-fields.md)
- [Custom Fields of Universal CRM Objects](./universal/userfieldconfig/index.md)
