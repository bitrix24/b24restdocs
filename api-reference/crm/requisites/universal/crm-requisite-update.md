# Update Requisite crm.requisite.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates an existing requisite.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the requisite, can be obtained using the [crm.requisite.list](./crm-requisite-list.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Set of requisite fields — an object of the form `"field": "value"[, ...]}`, the values of which need to be changed ||
|#

## Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the requisite ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key, used for exchange operations.

Identifier of the object in the external information base.

The purpose of the field may change by the final developer ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the external information base.

The purpose of the field may change by the final developer ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status.

Values `Y` or `N` are used.

Currently, the field does not actually affect anything ||
|| **ADDRESS_ONLY**
[`char`](../../../data-types.md) | Status indicating that the requisite is used only for storing the address.

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
[`string`](../../../data-types.md) | Taxpayer Identification Number ||
|| **RQ_KPP**
[`string`](../../../data-types.md) | Tax Registration Reason Code ||
|| **RQ_USRLE**
[`string`](../../../data-types.md) | Handelsregisternummer (for country DE) ||
|| **RQ_IFNS**
[`string`](../../../data-types.md) | Tax Authority ||
|| **RQ_OGRN**
[`string`](../../../data-types.md) | Primary State Registration Number ||
|| **RQ_OGRNIP**
[`string`](../../../data-types.md) | Individual Entrepreneur Registration Number ||
|| **RQ_OKPO**
[`string`](../../../data-types.md) | All-Russian Classifier of Enterprises and Organizations ||
|| **RQ_OKTMO**
[`string`](../../../data-types.md) | All-Russian Classifier of Territorial Units ||
|| **RQ_OKVED**
[`string`](../../../data-types.md) | All-Russian Classifier of Economic Activities ||
|| **RQ_EDRPOU**
[`string`](../../../data-types.md) | Unified State Register of Enterprises and Organizations ||
|| **RQ_DRFO**
[`string`](../../../data-types.md) | Tax Registration Number ||
|| **RQ_KBE**
[`string`](../../../data-types.md) | Classification of Business Entities ||
|| **RQ_IIN**
[`string`](../../../data-types.md) | Individual Identification Number ||
|| **RQ_BIN**
[`string`](../../../data-types.md) | Business Identification Number ||
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
[`string`](../../../data-types.md) | State Registration (IE) (for country BR) ||
|| **RQ_MNPL_REG**
[`string`](../../../data-types.md) | Municipal Registration (IM) (for country BR) ||
|| **RQ_CPF**
[`string`](../../../data-types.md) | CPF (for country BR) ||
|| **UF_CRM_...** | Custom fields. For example, `UF_CRM_1694526604`.

Requisites can have a set of custom fields with types: `string`, `boolean`, `double`, `datetime`.

You can add a custom field to requisites using the [crm.requisite.userfield.add](../user-fields/crm-requisite-userfield-add.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"fields":{"RQ_OKPO":"80715150","RQ_OKTMO":"45381000000","UF_CRM_1707997209":"78","UF_CRM_1708012333":"Category 3"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"fields":{"RQ_OKPO":"80715150","RQ_OKTMO":"45381000000","UF_CRM_1707997209":"78","UF_CRM_1708012333":"Category 3"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.update
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.update",
        {
            id: 27,
            fields:
            {
                "RQ_OKPO": "80715150",
                "RQ_OKTMO": "45381000000",
                "UF_CRM_1707997209": "78",
                "UF_CRM_1708012333": "Category 3"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.update',
        [
            'id' => 27,
            'fields' => [
                'RQ_OKPO' => '80715150',
                'RQ_OKTMO' => '45381000000',
                'UF_CRM_1707997209' => '78',
                'UF_CRM_1708012333' => 'Category 3'
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
    "result": true,
    "time": {
        "start": 1717163055.335685,
        "finish": 1717163055.892722,
        "duration": 0.5570368766784668,
        "processing": 0.17116189002990723,
        "date_start": "2024-05-31T15:44:15+02:00",
        "date_finish": "2024-05-31T15:44:15+02:00",
        "operating": 0.17112517356872559
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns the value:

- `true` — requisite changed
- `false` — requisite not changed

||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Response

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The Requisite with ID '57' is not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Error Text** | **Description** ||
|| Empty string | The Requisite with ID '57' is not found | The requisite with the specified identifier was not found ||
|| Empty string | ID is not defined or invalid. | The requisite identifier is not specified or has an invalid value ||
|| Empty string | ENTITY_TYPE_ID is not defined or invalid. | The identifier of the parent entity type is not specified or has an invalid value ||
|| Empty string | ENTITY_ID is not defined or invalid. | The identifier of the parent entity is not specified or has an invalid value ||
|| Empty string | PRESET_ID is not defined or invalid. | The identifier of the requisite template is not specified or has an invalid value ||
|| Empty string | Access denied. | Insufficient access permissions to modify the requisite ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-requisite-add.md)
- [{#T}](./crm-requisite-get.md)
- [{#T}](./crm-requisite-list.md)
- [{#T}](./crm-requisite-delete.md)
- [{#T}](./crm-requisite-fields.md)