# Get Requisite by ID crm.requisite.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves a requisite by its identifier `id`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the requisite.

The identifier can be obtained using the [crm.requisite.list](./crm-requisite-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.get
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.get",
        {
            id: 27
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
        'crm.requisite.get',
        [
            'id' => 27
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP Status: **200**

```json
{
    "result": {
        "ID": "27",
        "ENTITY_TYPE_ID": "4",
        "ENTITY_ID": "1",
        "PRESET_ID": "1",
        "DATE_CREATE": "2024-05-29T18:05:49+02:00",
        "DATE_MODIFY": "",
        "CREATED_BY_ID": "1",
        "MODIFY_BY_ID": null,
        "NAME": "Organization",
        "CODE": null,
        "XML_ID": "5e4641fd-1dd9-11e6-b2f2-005056c00008",
        "ORIGINATOR_ID": null,
        "ACTIVE": "Y",
        "ADDRESS_ONLY": "N",
        "SORT": "500",
        "RQ_NAME": null,
        "RQ_FIRST_NAME": null,
        "RQ_LAST_NAME": null,
        "RQ_SECOND_NAME": null,
        "RQ_COMPANY_ID": null,
        "RQ_COMPANY_NAME": "LLC \"1C-BITRIX\"",
        "RQ_COMPANY_FULL_NAME": "LIMITED LIABILITY COMPANY \"1C-BITRIX\"",
        "RQ_COMPANY_REG_DATE": "04.06.2007",
        "RQ_DIRECTOR": "RYZHIKOV SERGEY VLADIMIROVICH",
        "RQ_ACCOUNTANT": null,
        "RQ_CEO_NAME": null,
        "RQ_CEO_WORK_POS": null,
        "RQ_CONTACT": null,
        "RQ_EMAIL": null,
        "RQ_PHONE": null,
        "RQ_FAX": null,
        "RQ_IDENT_TYPE": null,
        "RQ_IDENT_DOC": null,
        "RQ_IDENT_DOC_SER": null,
        "RQ_IDENT_DOC_NUM": null,
        "RQ_IDENT_DOC_PERS_NUM": null,
        "RQ_IDENT_DOC_DATE": null,
        "RQ_IDENT_DOC_ISSUED_BY": null,
        "RQ_IDENT_DOC_DEP_CODE": null,
        "RQ_INN": "7717586110",
        "RQ_KPP": "770501001",
        "RQ_USRLE": null,
        "RQ_IFNS": null,
        "RQ_OGRN": "5077746476209",
        "RQ_OGRNIP": null,
        "RQ_OKPO": null,
        "RQ_OKTMO": null,
        "RQ_OKVED": null,
        "RQ_EDRPOU": null,
        "RQ_DRFO": null,
        "RQ_KBE": null,
        "RQ_IIN": null,
        "RQ_BIN": null,
        "RQ_ST_CERT_SER": null,
        "RQ_ST_CERT_NUM": null,
        "RQ_ST_CERT_DATE": null,
        "RQ_VAT_PAYER": "N",
        "RQ_VAT_ID": null,
        "RQ_VAT_CERT_SER": null,
        "RQ_VAT_CERT_NUM": null,
        "RQ_VAT_CERT_DATE": null,
        "RQ_RESIDENCE_COUNTRY": null,
        "RQ_BASE_DOC": null,
        "RQ_REGON": null,
        "RQ_KRS": null,
        "RQ_PESEL": null,
        "RQ_LEGAL_FORM": null,
        "RQ_SIRET": null,
        "RQ_SIREN": null,
        "RQ_CAPITAL": null,
        "RQ_RCS": null,
        "RQ_CNPJ": null,
        "RQ_STATE_REG": null,
        "RQ_MNPL_REG": null,
        "RQ_CPF": null
    },
    "time": {
        "start": 1717078089.78188,
        "finish": 1717078090.270494,
        "duration": 0.4886140823364258,
        "processing": 0.09770989418029785,
        "date_start": "2024-05-30T16:08:09+02:00",
        "date_finish": "2024-05-30T16:08:10+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`Object`| An object containing the values of [requisite fields](./index.md#fields) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Response

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The Requisite with ID '27' is not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The Requisite with ID '27' is not found | The requisite with the specified identifier was not found ||
|| Empty string | Access denied. | Insufficient access permissions to retrieve the requisite ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-requisite-add.md)
- [{#T}](./crm-requisite-update.md)
- [{#T}](./crm-requisite-list.md)
- [{#T}](./crm-requisite-delete.md)
- [{#T}](./crm-requisite-fields.md)