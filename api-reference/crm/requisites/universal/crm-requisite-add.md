# Add Requisite crm.requisite.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a new requisite.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Set of fields — an object of the form `{"field": "value"[, ...]}` for adding the requisite ||
|#

## Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the parent entity type. 

Currently, this can only be:
- `3` — contact
- `4` — company

Identifiers for all CRM entity types can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md)
||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Identifier of the parent entity (contact or company).

The identifier can be obtained using the method [crm.company.list](../../companies/crm-company-list.md) for companies and the method [crm.contact.list](../../contacts/crm-contact-list.md) for contacts ||
|| **PRESET_ID***
[`integer`](../../../data-types.md) | Identifier of the requisite template.

Template identifiers can be obtained using the method [crm.requisite.preset.list](../presets/crm-requisite-preset-list.md) ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the requisite ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key, used for exchange operations.

Identifier of the external information base object.

The purpose of the field may change by the final developer ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the external information base.

The purpose of the field may change by the final developer ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status.

Values `Y` or `N` are used.

Currently, the field does not actually affect anything ||
|| **ADDRESS_ONLY**
[`char`](../../../data-types.md) | Status indicator when the requisite is used only for storing the address.

Values `Y` or `N` are used. When set to `Y`, the requisites are not displayed in the entity card, but the address is shown ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting.

Order in the list of entity requisites when there are multiple ||
|| **RQ_NAME**
[`string`](../../../data-types.md) | Full name ||
|| **RQ_FIRST_NAME**
[`string`](../../../data-types.md) | First name ||
|| **RQ_LAST_NAME**
[`string`](../../../data-types.md) | Last name ||
|| **RQ_SECOND_NAME**
[`string`](../../../data-types.md) | Middle name ||
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
[`crm_status`](../../../data-types.md) | Identification method ||
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
[`string`](../../../data-types.md) | Tax ID ||
|| **RQ_KPP**
[`string`](../../../data-types.md) | Tax registration reason code ||
|| **RQ_USRLE**
[`string`](../../../data-types.md) | Commercial register number (for country DE) ||
|| **RQ_IFNS**
[`string`](../../../data-types.md) | Tax authority ||
|| **RQ_OGRN**
[`string`](../../../data-types.md) | Primary state registration number ||
|| **RQ_OGRNIP**
[`string`](../../../data-types.md) | Primary state registration number for individual entrepreneurs ||
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
[`string`](../../../data-types.md) | Series of the state registration certificate ||
|| **RQ_ST_CERT_NUM**
[`string`](../../../data-types.md) | Number of the state registration certificate ||
|| **RQ_ST_CERT_DATE**
[`string`](../../../data-types.md) | Date of the state registration certificate ||
|| **RQ_VAT_PAYER**
[`char`](../../../data-types.md) | VAT payer (for country UA).

Values `Y` or `N` are used ||
|| **RQ_VAT_ID**
[`string`](../../../data-types.md) | VAT ID (identification number of the VAT payer) ||
|| **RQ_VAT_CERT_SER**
[`string`](../../../data-types.md) | Series of the VAT certificate ||
|| **RQ_VAT_CERT_NUM**
[`string`](../../../data-types.md) | Number of the VAT certificate ||
|| **RQ_VAT_CERT_DATE**
[`string`](../../../data-types.md) | Date of the VAT certificate ||
|| **RQ_RESIDENCE_COUNTRY**
[`string`](../../../data-types.md) | Country of residence ||
|| **RQ_BASE_DOC**
[`string`](../../../data-types.md) | Basis for the action ||
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
[`string`](../../../data-types.md) | State registration (IE) (for country BR) ||
|| **RQ_MNPL_REG**
[`string`](../../../data-types.md) | Municipal registration (IM) (for country BR) ||
|| **RQ_CPF**
[`string`](../../../data-types.md) | CPF (for country BR) ||
|| **UF_CRM_...** | Custom fields. For example, `UF_CRM_1694526604`.

Requisites can have a set of custom fields with types: `string`, `boolean`, `double`, `datetime`.

You can add a custom field to the requisites using the method [crm.requisite.userfield.add](../user-fields/crm-requisite-userfield-add.md) ||
|#

{% note info "Which fields with the prefix `RQ_` can be specified?" %}

When creating a requisite, only those fields with the prefix `RQ_` that are present in the requisite template linked to the created requisite (see the field `PRESET_ID`) should be specified. The values of other fields will be saved but will not be visible to the user.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_TYPE_ID":4,"ENTITY_ID":1,"PRESET_ID":1,"NAME":"Organization","ACTIVE":"Y","ADDRESS_ONLY":"N","SORT":500,"RQ_COMPANY_NAME":"Ltd. \"Bitrix24\"","RQ_COMPANY_FULL_NAME":"LIMITED LIABILITY COMPANY \"Bitrix24\"","RQ_COMPANY_REG_DATE":"06.04.2007","RQ_DIRECTOR":"SMITH JOHN DOE","RQ_INN":"7717586110","RQ_KPP":"770501001","RQ_OGRN":"5077746476209","UF_CRM_1707997209":"56","UF_CRM_1708012333":"Category 1","XML_ID":"5e4641fd-1dd9-11e6-b2f2-005056c00008"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_TYPE_ID":4,"ENTITY_ID":1,"PRESET_ID":1,"NAME":"Organization","ACTIVE":"Y","ADDRESS_ONLY":"N","SORT":500,"RQ_COMPANY_NAME":"Ltd. \"Bitrix24\"","RQ_COMPANY_FULL_NAME":"LIMITED LIABILITY COMPANY \"Bitrix24\"","RQ_COMPANY_REG_DATE":"06.04.2007","RQ_DIRECTOR":"SMITH JOHN DOE","RQ_INN":"7717586110","RQ_KPP":"770501001","RQ_OGRN":"5077746476209","UF_CRM_1707997209":"56","UF_CRM_1708012333":"Category 1","XML_ID":"5e4641fd-1dd9-11e6-b2f2-005056c00008","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.add
    ```

- JS

    ```javascript
    BX24.callMethod(
        "crm.requisite.add",
        {
            fields:
            {
                "ENTITY_TYPE_ID": 4,
                "ENTITY_ID": 1,
                "PRESET_ID": 1,
                "NAME": "Organization",
                "ACTIVE": "Y",
                "ADDRESS_ONLY": "N",
                "SORT": 500,
                "RQ_COMPANY_NAME": "Ltd. \"Bitrix24\"",
                "RQ_COMPANY_FULL_NAME": "LIMITED LIABILITY COMPANY \"Bitrix24\"",
                "RQ_COMPANY_REG_DATE": "06.04.2007",
                "RQ_DIRECTOR": "SMITH JOHN DOE",
                "RQ_INN": "7717586110",
                "RQ_KPP": "770501001",
                "RQ_OGRN": "5077746476209",
                "UF_CRM_1707997209": "56",
                "UF_CRM_1708012333": "Category 1",
                "XML_ID": "5e4641fd-1dd9-11e6-b2f2-005056c00008"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Requisite created with ID " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.add',
        [
            'fields' => [
                "ENTITY_TYPE_ID" => 4,
                "ENTITY_ID" => 1,
                "PRESET_ID" => 1,
                "NAME" => "Organization",
                "ACTIVE" => "Y",
                "ADDRESS_ONLY" => "N",
                "SORT" => 500,
                "RQ_COMPANY_NAME" => "Ltd. \"Bitrix24\"",
                "RQ_COMPANY_FULL_NAME" => "LIMITED LIABILITY COMPANY \"Bitrix24\"",
                "RQ_COMPANY_REG_DATE" => "06.04.2007",
                "RQ_DIRECTOR" => "SMITH JOHN DOE",
                "RQ_INN" => "7717586110",
                "RQ_KPP" => "770501001",
                "RQ_OGRN" => "5077746476209",
                "UF_CRM_1707997209" => "56",
                "UF_CRM_1708012333" => "Category 1",
                "XML_ID" => "5e4641fd-1dd9-11e6-b2f2-005056c00008"
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}


## Successful Response

HTTP status: **200**

```json
{
    "result": 27,
    "time": {
        "start": 1716998748.040801,
        "finish": 1716998749.444508,
        "duration": 1.4037070274353027,
        "processing": 0.32904696464538574,
        "date_start": "2024-05-29T18:05:48+02:00",
        "date_finish": "2024-05-29T18:05:49+02:00",
        "operating": 0.32897305488586426
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created requisite ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Response

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "ENTITY_TYPE_ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Error Text** | **Description** ||
|| Empty string | ENTITY_TYPE_ID is not defined or invalid. | Identifier of the parent entity type is not defined or has an invalid value ||
|| Empty string | ENTITY_ID is not defined or invalid. | Identifier of the parent entity is not defined or has an invalid value ||
|| Empty string | PRESET_ID is not defined or invalid. | Identifier of the requisite template is not defined or has an invalid value ||
|| Empty string | Entity not found. | The entity for which the requisite is being created was not found ||
|| Empty string | Access denied. | Insufficient access permissions to add the requisite ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-requisite-update.md)
- [{#T}](./crm-requisite-get.md)
- [{#T}](./crm-requisite-list.md)
- [{#T}](./crm-requisite-delete.md)
- [{#T}](./crm-requisite-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-company-with-requisite.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contact-with-requisite.md)