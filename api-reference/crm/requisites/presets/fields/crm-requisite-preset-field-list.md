# Get a list of all customizable fields for a specified CRM requisites template crm.requisite.preset.field.list

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of all customizable fields for a specific requisites template.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **preset***
[`object`](../../../../data-types.md) | An object containing the identifier value of the template from which the list of customizable fields is extracted (for example, `{"ID": 27}`). Template identifiers can be obtained using the [crm.requisite.preset.list](../crm-requisite-preset-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"preset":{"ID":27}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.list
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"preset":{"ID":27},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.preset.field.list",
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
        'crm.requisite.preset.field.list',
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

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": 1,
            "FIELD_NAME": "RQ_INN",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "Y",
            "SORT": 510
        },
        {
            "ID": 2,
            "FIELD_NAME": "RQ_COMPANY_NAME",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "Y",
            "SORT": 520
        },
        {
            "ID": 3,
            "FIELD_NAME": "RQ_COMPANY_FULL_NAME",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 530
        },
        {
            "ID": 4,
            "FIELD_NAME": "RQ_OGRN",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 540
        },
        {
            "ID": 5,
            "FIELD_NAME": "RQ_KPP",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "Y",
            "SORT": 550
        },
        {
            "ID": 6,
            "FIELD_NAME": "RQ_COMPANY_REG_DATE",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 560
        },
        {
            "ID": 7,
            "FIELD_NAME": "RQ_OKPO",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 570
        },
        {
            "ID": 8,
            "FIELD_NAME": "RQ_OKTMO",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 580
        },
        {
            "ID": 9,
            "FIELD_NAME": "RQ_DIRECTOR",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 590
        },
        {
            "ID": 10,
            "FIELD_NAME": "RQ_ACCOUNTANT",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 600
        },
        {
            "ID": 11,
            "FIELD_NAME": "RQ_ADDR",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 610
        },
        {
            "ID": 12,
            "FIELD_NAME": "RQ_SIGNATURE",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 620
        },
        {
            "ID": 13,
            "FIELD_NAME": "RQ_STAMP",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 630
        },
        {
            "ID": 14,
            "FIELD_NAME": "UF_CRM_1703689889",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "Y",
            "SORT": 640
        },
        {
            "ID": 15,
            "FIELD_NAME": "UF_CRM_1703690003",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "Y",
            "SORT": 650
        }
    ],
    "time": {
        "start": 1716895759.934609,
        "finish": 1716895760.407579,
        "duration": 0.47297000885009766,
        "processing": 0.023286104202270508,
        "date_start": "2024-05-28T13:29:19+02:00",
        "date_finish": "2024-05-28T13:29:20+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md)| An array of objects describing the customizable fields of the requisites template. Each element contains [fields](#fields) of the customizable field template ||
|| **total**
[`integer`](../../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../../data-types.md) | Information about the execution time of the request ||
|#

### Fields Describing the Customizable Field of the Requisites Template {#fields}

#|
||  **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the field. Created automatically and unique within the template ||
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Name of the field ||
|| **FIELD_TITLE**
[`string`](../../../../data-types.md) | Alternative name of the field for the requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used 
||
|| **SORT**
[`integer`](../../../../data-types.md) | Sorting. Order in the list of template fields || 
|| **IN_SHORT_LIST**
[`char`](../../../../data-types.md) | Show in the short list. Deprecated field, currently not used. Retained for backward compatibility. Can take values `Y` or `N` ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The Preset with ID '27' is not found"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `The Preset with ID '27' is not found` | The template with the specified identifier was not found ||
|| `Access denied` | Insufficient access permissions to retrieve the list of customizable fields of the template ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-add.md)
- [{#T}](./crm-requisite-preset-field-update.md)
- [{#T}](./crm-requisite-preset-field-available-to-add.md)
- [{#T}](./crm-requisite-preset-field-get.md)
- [{#T}](./crm-requisite-preset-field-delete.md)
- [{#T}](./crm-requisite-preset-field-fields.md)