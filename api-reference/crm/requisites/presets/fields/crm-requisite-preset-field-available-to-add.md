# Get Fields Available for Addition to the CRM Requisite Template crm.requisite.preset.field.availabletoadd

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns the fields that can be added to the specified requisite template.

## Method Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **preset***
[`object`](../../../../data-types.md) | An object containing the identifier value of the template for which you want to get the list of available customizable fields. 

Template identifiers can be obtained using the [crm.requisite.preset.list](../crm-requisite-preset-list.md) method. 

Fields with the prefix `UF_` in the response are custom fields (see [methods](../../user-fields/index.md) for working with custom requisite fields) ||
|#

## Code Examples

{% include [Note on Examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"preset":{"ID":27}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.availabletoadd
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"preset":{"ID":27},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.availabletoadd
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.preset.field.availabletoadd",
        {
            preset:
            {
                "ID": 27
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.preset.field.availabletoadd',
        [
            'preset' => ['ID' => 27]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        "RQ_FIRST_NAME",
        "RQ_LAST_NAME",
        "RQ_SECOND_NAME",
        "RQ_COMPANY_NAME",
        "RQ_COMPANY_FULL_NAME",
        "RQ_COMPANY_REG_DATE",
        "RQ_DIRECTOR",
        "RQ_ACCOUNTANT",
        "RQ_ADDR",
        "RQ_CONTACT",
        "RQ_EMAIL",
        "RQ_PHONE",
        "RQ_FAX",
        "RQ_IDENT_DOC",
        "RQ_IDENT_DOC_SER",
        "RQ_IDENT_DOC_NUM",
        "RQ_IDENT_DOC_DATE",
        "RQ_IDENT_DOC_ISSUED_BY",
        "RQ_IDENT_DOC_DEP_CODE",
        "RQ_INN",
        "RQ_KPP",
        "RQ_IFNS",
        "RQ_OGRN",
        "RQ_OGRNIP",
        "RQ_OKPO",
        "RQ_OKTMO",
        "RQ_OKVED",
        "RQ_ST_CERT_SER",
        "RQ_ST_CERT_NUM",
        "RQ_ST_CERT_DATE",
        "RQ_SIGNATURE",
        "RQ_STAMP",
        "UF_CRM_1707997209",
        "UF_CRM_1707997236",
        "UF_CRM_1707997253",
        "UF_CRM_1708012333"
    ]
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md) | An array of field names that can be added to the specified requisite template ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

### Field Descriptions

#|
|| **Name**
`type` | **Description** ||
|| **RQ_FIRST_NAME**
[`string`](../../../../data-types.md) | First Name ||
|| **RQ_LAST_NAME**
[`string`](../../../../data-types.md) | Last Name ||
|| **RQ_SECOND_NAME**
[`string`](../../../../data-types.md) | Middle Name ||
|| **RQ_COMPANY_NAME**
[`string`](../../../../data-types.md) | Short Name of the Organization ||
|| **RQ_COMPANY_FULL_NAME**
[`string`](../../../../data-types.md) | Full Name of the Organization ||
|| **RQ_COMPANY_REG_DATE**
[`string`](../../../../data-types.md) | Date of State Registration ||
|| **RQ_DIRECTOR**
[`string`](../../../../data-types.md) | General Director ||
|| **RQ_ACCOUNTANT**
[`string`](../../../../data-types.md) | Chief Accountant ||
|| **RQ_CONTACT**
[`string`](../../../../data-types.md) | Contact Person ||
|| **RQ_EMAIL**
[`string`](../../../../data-types.md) | E-Mail ||
|| **RQ_PHONE**
[`string`](../../../../data-types.md) | Phone ||
|| **RQ_FAX**
[`string`](../../../../data-types.md) | Fax ||
|| **RQ_IDENT_DOC**
[`string`](../../../../data-types.md) | Document Type ||
|| **RQ_IDENT_DOC_SER**
[`string`](../../../../data-types.md) | Series ||
|| **RQ_IDENT_DOC_NUM**
[`string`](../../../../data-types.md) | Number ||
|| **RQ_IDENT_DOC_DATE**
[`string`](../../../../data-types.md) | Issue Date ||
|| **RQ_IDENT_DOC_ISSUED_BY**
[`string`](../../../../data-types.md) | Issued By ||
|| **RQ_IDENT_DOC_DEP_CODE**
[`string`](../../../../data-types.md) | Department Code ||
|| **RQ_INN**
[`string`](../../../../data-types.md) | Tax Identification Number ||
|| **RQ_KPP**
[`string`](../../../../data-types.md) | Tax Registration Reason Code ||
|| **RQ_IFNS**
[`string`](../../../../data-types.md) | Tax Authority Code ||
|| **RQ_OGRN**
[`string`](../../../../data-types.md) | Primary State Registration Number ||
|| **RQ_OGRNIP**
[`string`](../../../../data-types.md) | Individual Entrepreneur Registration Number ||
|| **RQ_OKPO**
[`string`](../../../../data-types.md) | All-Russian Classifier of Enterprises and Organizations Code ||
|| **RQ_OKTMO**
[`string`](../../../../data-types.md) | All-Russian Classifier of Territories Code ||
|| **RQ_OKVED**
[`string`](../../../../data-types.md) | All-Russian Classifier of Economic Activities Code ||
|| **RQ_ST_CERT_SER**
[`string`](../../../../data-types.md) | Series of State Registration Certificate ||
|| **RQ_ST_CERT_NUM**
[`string`](../../../../data-types.md) | Number of State Registration Certificate ||
|| **RQ_ST_CERT_DATE**
[`string`](../../../../data-types.md) | Date of State Registration Certificate ||
|| **UF_CRM_1707997209**
[`double`](../../../../data-types.md) | Custom Field of Type "Number" ||
|| **UF_CRM_1707997236**
[`boolean`](../../../../data-types.md) | Custom Field of Type "Yes/No" ||
|| **UF_CRM_1707997253**
[`datetime`](../../../../data-types.md) | Custom Field of Type "Date" ||
|| **UF_CRM_1708012333**
[`string`](../../../../data-types.md) | Custom Field of Type "String" ||
|#

## Error Handling

HTTP Status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "Template not found."
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `Template not found` | The template for which the list of fields available for addition could not be found ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-add.md)
- [{#T}](./crm-requisite-preset-field-update.md)
- [{#T}](./crm-requisite-preset-field-get.md)
- [{#T}](./crm-requisite-preset-field-list.md)
- [{#T}](./crm-requisite-preset-field-delete.md)
- [{#T}](./crm-requisite-preset-field-fields.md)