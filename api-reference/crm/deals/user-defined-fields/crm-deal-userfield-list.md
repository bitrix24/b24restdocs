# Get a list of custom fields for deals crm.deal.userfield.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

The method `crm.deal.userfield.list` returns a list of custom fields for deals based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`object`](../../../data-types.md) | Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — the name of the field by which the selection of custom fields will be filtered
- `value_n` — the filter value

All conditions for individual fields are combined using `AND`. See the [list of available fields for filtering](#filterable) below. ||
|| **order**
[`object`](../../../data-types.md) | Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — the name of the field by which the selection of elements will be sorted
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

Available fields for sorting:
- `ID` — identifier of the custom field
- `FIELD_NAME` — code of the custom field
- `USER_TYPE_ID` — type of the custom field
- `XML_ID` — external code
- `SORT` — sort index

By default:
```
{
    "SORT": "ASC",
    "ID": "ASC"
}
```
||
|#

### Available fields for filtering {#filterable}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the custom field ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Code of the custom field ||
|| **USER_TYPE_ID**
[`string`](../../../data-types.md) | Type of the custom field. Possible values:
- `string` — string
- `integer` — integer
- `double` — number
- `boolean` — yes/no
- `datetime` — date/time
- `date` — date
- `money` — money
- `url` — link
- `address` — address
- `enumeration` — list
- `file` — file
- `employee` — binding to an employee
- `crm_status` — binding to the CRM directory
- `iblock_section` — binding to information block sections
- `iblock_element` — binding to information block elements
- `crm` — binding to CRM elements
- [custom field types](../../universal/user-defined-fields/userfield-type.md)
||
|| **XML_ID**
[`string`](../../../data-types.md) | External code ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index ||
|| **MULTIPLE**
[`boolean`](../../../data-types.md) | Whether the custom field is multiple.
Possible values:
- `Y` — yes
- `N` — no ||
|| **MANDATORY**
[`boolean`](../../../data-types.md) | Whether the custom field is mandatory. Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Whether to show in the list filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring ||
|| **SHOW_IN_LIST**
[`boolean`](../../../data-types.md) | Whether to show in the list. Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_IN_LIST**
[`boolean`](../../../data-types.md) | Whether to allow user editing. Possible values:
- `Y` — yes
- `N` — no ||
|| **IS_SEARCHABLE**
[`boolean`](../../../data-types.md) | Whether the field values are searchable. Possible values:
- `Y` — yes
- `N` — no ||
|| **LANG**
[`string`](../../../data-types.md) | [Language identifier](../../data-types.md#lang-ids). When filtering by this parameter, a set of fields with values in the provided language will be returned:
- `EDIT_FORM_LABEL` — label in the edit form
- `LIST_COLUMN_LABEL` — header in the list
- `LIST_FILTER_LABEL` — filter label in the list
- `ERROR_MESSAGE` — error message
- `HELP_MESSAGE` — help ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Get a list of custom fields that:
- are multiple,
- are mandatory,
- have custom field labels in German. Thanks to the filter by the `LANG` parameter, we will additionally receive field names in the response.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"MULTIPLE":"Y","MANDATORY":"Y","LANG":"de"},"order":{"USER_TYPE_ID":"ASC","SORT":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/crm.deal.userfield.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"MULTIPLE":"Y","MANDATORY":"Y","LANG":"de"},"order":{"USER_TYPE_ID":"ASC","SORT":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.userfield.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve
    // the entire set of list data and the volume of records is relatively small
    // (up to about 1000 items). The method loads all data at once, which
    // can lead to high memory load when working with large volumes.
    
    try {
    const response = await $b24.callListMethod(
        'crm.deal.userfield.list',
        {
         filter: {
            MULTIPLE: "Y",
            MANDATORY: "Y",
            LANG: "de",
         },
         order: {
            USER_TYPE_ID: "ASC",
            SORT: "ASC",
         },
        },
        (progress: number) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets.
    // The method implements iterative selection using a generator, which
    // allows processing data in parts and efficiently using memory.
    
    try {
    const generator = $b24.fetchListMethod('crm.deal.userfield.list', {
        filter: {
         MULTIPLE: "Y",
         MANDATORY: "Y",
         LANG: "de",
        },
        order: {
         USER_TYPE_ID: "ASC",
         SORT: "ASC",
        },
    }, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error: any) {
    console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination
    // of data retrieval through the start parameter. Suitable for scenarios where
    // precise control over request batches is required. However, with large
    // volumes of data, it may be less efficient compared to
    // fetchListMethod.
    
    try {
    const response = await $b24.callMethod('crm.deal.userfield.list', {
        filter: {
         MULTIPLE: "Y",
         MANDATORY: "Y",
         LANG: "de",
        },
        order: {
         USER_TYPE_ID: "ASC",
         SORT: "ASC",
        },
    }, 0);
    const result = response.getData().result || [];
    for (const entity of result) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.userfield.list',
                [
                    'filter' => [
                        'MULTIPLE' => 'Y',
                        'MANDATORY' => 'Y',
                        'LANG' => 'de',
                    ],
                    'order' => [
                        'USER_TYPE_ID' => 'ASC',
                        'SORT' => 'ASC',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching deal user fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.userfield.list',
        {
            filter: {
                MULTIPLE: "Y",
                MANDATORY: "Y",
                LANG: "de",
            },
            order: {
                USER_TYPE_ID: "ASC",
                SORT: "ASC",
            },
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.userfield.list',
        [
            'filter' => [
                'MULTIPLE' => "Y",
                'MANDATORY' => "N",
                'LANG' => "de",
            ],
            'order' => [
                'USER_TYPE_ID' => "ASC",
                'SORT' => "ASC",
            ]
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
            "ID": "5815",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1713790573",
            "USER_TYPE_ID": "crm_status",
            "XML_ID": null,
            "SORT": "100",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "I",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "ENTITY_TYPE": "INDUSTRY"
            },
            "EDIT_FORM_LABEL": "Directory",
            "LIST_COLUMN_LABEL": "Directory",
            "LIST_FILTER_LABEL": "Directory",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "6799",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1724077760",
            "USER_TYPE_ID": "enumeration",
            "XML_ID": null,
            "SORT": "150",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "E",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "DISPLAY": "LIST",
                "LIST_HEIGHT": 1,
                "CAPTION_NO_VALUE": "",
                "SHOW_NO_VALUE": "Y"
            },
            "EDIT_FORM_LABEL": "Radio",
            "LIST_COLUMN_LABEL": "Radio",
            "LIST_FILTER_LABEL": "Radio",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null,
            "LIST": [
                {
                    "ID": "3157",
                    "SORT": "10",
                    "VALUE": "Children's Radio",
                    "DEF": "N",
                    "XML_ID": "79b4c576f96e65eb40f390e45c0dc802"
                },
                {
                    "ID": "3159",
                    "SORT": "20",
                    "VALUE": "Shanson Radio",
                    "DEF": "N",
                    "XML_ID": "d3ffd89a825f218f5efd79dffd38fbbf"
                },
                {
                    "ID": "3161",
                    "SORT": "30",
                    "VALUE": "Love Radio",
                    "DEF": "N",
                    "XML_ID": "dff769fa19d7d7ce8d0677341d221161"
                },
                {
                    "ID": "3163",
                    "SORT": "40",
                    "VALUE": "Monte Carlo Radio",
                    "DEF": "N",
                    "XML_ID": "1f22e5c818ce336a42bd0ec94eb69b9e"
                },
                {
                    "ID": "3181",
                    "SORT": "130",
                    "VALUE": "DFM Yuriev-Polsky",
                    "DEF": "N",
                    "XML_ID": "fc1c5e6b4a9fd20b4749240b8dbac41a"
                }
            ]
        },
        {
            "ID": "6791",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1722578010",
            "USER_TYPE_ID": "file",
            "XML_ID": null,
            "SORT": "150",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "E",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "SIZE": 20,
                "LIST_WIDTH": 0,
                "LIST_HEIGHT": 0,
                "MAX_SHOW_SIZE": 0,
                "MAX_ALLOWED_SIZE": 0,
                "EXTENSIONS": [],
                "TARGET_BLANK": "Y"
            },
            "EDIT_FORM_LABEL": "Estimate (files)",
            "LIST_COLUMN_LABEL": "Estimate (files)",
            "LIST_FILTER_LABEL": "Estimate (files)",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "5567",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1709565075",
            "USER_TYPE_ID": "resourcebooking",
            "XML_ID": null,
            "SORT": "1",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "USE_USERS": "N",
                "USE_RESOURCES": "Y",
                "RESOURCES": {
                    "resource": {
                        "XML_ID": "resource",
                        "NAME": "resource",
                        "SECTIONS": [
                            {
                                "ID": "7",
                                "CAL_TYPE": "resource",
                                "NAME": "Museum Hall"
                            },
                            {
                                "ID": "49",
                                "CAL_TYPE": "resource",
                                "NAME": "resource 1"
                            },
                            {
                                "ID": "51",
                                "CAL_TYPE": "resource",
                                "NAME": "resource 2"
                            },
                            {
                                "ID": "53",
                                "CAL_TYPE": "resource",
                                "NAME": "1"
                            },
                            {
                                "ID": "57",
                                "CAL_TYPE": "resource",
                                "NAME": "Resource"
                            },
                            {
                                "ID": "59",
                                "CAL_TYPE": "resource",
                                "NAME": "Resource 2"
                            }
                        ]
                    }
                },
                "SELECTED_RESOURCES": [
                    {
                        "type": "resource",
                        "id": "7",
                        "title": "Museum Hall"
                    }
                ],
                "SELECTED_USERS": [],
                "FULL_DAY": "N",
                "ALLOW_OVERBOOKING": "Y",
                "USE_SERVICES": "N",
                "SERVICE_LIST": [
                    {
                        "name": "",
                        "duration": "60"
                    }
                ],
                "RESOURCE_LIMIT": -1,
                "TIMEZONE": "Europe/Berlin",
                "USE_USER_TIMEZONE": "N"
            },
            "EDIT_FORM_LABEL": "MEETING ROOM BOOKING",
            "LIST_COLUMN_LABEL": "MEETING ROOM BOOKING",
            "LIST_FILTER_LABEL": "MEETING ROOM BOOKING",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "6997",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_HELLO_WORLD",
            "USER_TYPE_ID": "string",
            "XML_ID": null,
            "SORT": "2000",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "N",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "SIZE": 20,
                "ROWS": 10,
                "REGEXP": "",
                "MIN_LENGTH": 0,
                "MAX_LENGTH": 0,
                "DEFAULT_VALUE": "Hello, world! Default value (modified)"
            },
            "EDIT_FORM_LABEL": "Hello, world! Edit (modified)",
            "LIST_COLUMN_LABEL": "Hello, world! Column (modified)",
            "LIST_FILTER_LABEL": "Hello, world! Filter (modified)",
            "ERROR_MESSAGE": "Hello, world! Error (modified)",
            "HELP_MESSAGE": "Hello, world! Help (modified)"
        }
    ],
    "total": 5,
    "time": {
        "start": 1753793143.219832,
        "finish": 1753793143.529472,
        "duration": 0.30964016914367676,
        "processing": 0.06361007690429688,
        "date_start": "2025-07-29T15:45:43+02:00",
        "date_finish": "2025-07-29T15:45:43+02:00",
        "operating_reset_at": 1753793743,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The root element of the response, contains a list of custom fields.

The structure of an individual custom field depends on its type. The fields `EDIT_FORM_LABEL`, `LIST_COLUMN_LABEL`, `LIST_FILTER_LABEL`, `ERROR_MESSAGE`, `HELP_MESSAGE` are returned either as `string` when passing `filter.LANG`, or are not returned at all. ||
|| **total**
[`integer`](../../../data-types.md) | The number of found custom fields ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'filter' must be array."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `Parameter 'order' must be array` | The provided `order` is not an object ||
|| `400`     | `Parameter 'filter' must be array` | The provided `filter` is not an object ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-userfield-add.md)
- [{#T}](./crm-deal-userfield-update.md)
- [{#T}](./crm-deal-userfield-get.md)
- [{#T}](./crm-deal-userfield-delete.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-precision-to-user-field.md)
- [{#T}](../../../../tutorials/crm/how-to-edit-crm-objects/how-to-set-paid-date-to-deal.md)