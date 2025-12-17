# Update the Universal List Field lists.field.update

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Full access" permission for the required list

The method `lists.field.update` updates a list field.

{% note warning "" %}

The field type cannot be changed. Please provide the type that was specified when the field was created.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_TYPE_ID***
[`string`](../../data-types.md) | Identifier of the information block type. Possible values: 
- `lists` — list information block type 
- `bitrix_processes` — processes information block type 
- `lists_socnet` — group lists information block type 
  
The identifier can be obtained using the [lists.get.iblock.type.id](../lists/lists-get-iblock-type-id.md) method. ||
|| **IBLOCK_ID***
[`integer`](../../data-types.md) | Identifier of the information block.

The identifier can be obtained using the [lists.get](../lists/lists-get.md) method. ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the [lists.get](../lists/lists-get.md) method.

{% note info "" %}

At least one of the parameters: `IBLOCK_ID` or `IBLOCK_CODE` must be specified.

{% endnote %} ||
|| **FIELDS***
[`array`](../../data-types.md) | Array of parameters.

[Detailed description](#parametr-fields) ||
|| **FIELD_ID***
[`string`](../../data-types.md) | Identifier of the field. For a custom field, it appears as `PROPERTY_PropertyId`. For a system field, it is its symbolic code.

The identifier can be obtained using the [lists.field.get](./lists-field-get.md) method. ||
|#

### Parameter FIELDS {#parametr-fields}

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../data-types.md) | Field name ||
|| **TYPE***
[`string`](../../data-types.md) | Field type. The type cannot be changed; please provide the one that was specified when the field was created.

Custom fields:
- `S` — String
- `N` — Number
- `L` — List
- `F` — File
- `G` — Section binding
- `E` — Element binding
- `S:Date` — Date
- `S:DateTime` — Date/Time
- `S:HTML` — HTML/text
- `E:EList` — Element binding in list form
- `N:Sequence` — Counter
- `S:ECrm` — CRM element binding 
- `S:Money` — Money
- `S:DiskFile` — File (Disk)
- `S:employee` — Employee binding

System fields:
- `SORT` — Sorting
- `ACTIVE_FROM` — Start of activity
- `ACTIVE_TO` — End of activity
- `PREVIEW_PICTURE` — Preview image
- `PREVIEW_TEXT` — Preview text
- `DETAIL_PICTURE` — Detailed image
- `DETAIL_TEXT` — Detailed text
- `DATE_CREATE` — Creation date
- `CREATED_BY` — Created by
- `TIMESTAMP_X` — Modification date
- `MODIFIED_BY` — Modified by

Values in the fields Creation date, Modification date, Created by, and Modified by are filled automatically. ||
|| **IS_REQUIRED**
[`string`](../../data-types.md) | Field required flag. Possible values:
- `Y` — yes
- `N` — no ||
|| **MULTIPLE**
[`string`](../../data-types.md) | Field multiplicity flag. Possible values:
- `Y` — yes
- `N` — no  

System fields cannot be multiple. ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting ||
|| **DEFAULT_VALUE** | Default value. If the `ADD_READ_ONLY_FIELD` setting is enabled in `SETTINGS`, this parameter becomes required. ||
|| **LIST**
[`array`](../../data-types.md) | Values for the List type field. An array in the format `{'ID': { 'VALUE': 'Value_1', 'SORT': 10, 'DEF': 'N' }}`, where `'ID'` — identifier, `VALUE` — displayed text, `SORT` — sorting order, `DEF` — default value indicator.

The identifier can be obtained using the [lists.field.get](./lists-field-get.md) method. ||
|| **LIST_TEXT_VALUES**
[`string`](../../data-types.md) | An alternative way to specify values for the List type field. A string where values are separated by the newline character `\n`.

{% note info "" %}

When updating a List type field, you can use either `LIST`, `LIST_TEXT_VALUES`, or both parameters together. In this case, the values from the string will be added to those already in the array.

{% endnote %} ||
|| **LIST_DEF**
[`array`](../../data-types.md) | Default value for the List type field. The array accepts the identifier from `LIST`. ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the field. Required for custom fields. Not used for system fields. ||
|| **SETTINGS**
[`array`](../../data-types.md) | Display and behavior settings.

Supported values:
- `SHOW_ADD_FORM` — show in the add form
- `SHOW_EDIT_FORM` — show in the edit form
- `ADD_READ_ONLY_FIELD` — read-only (add form)
- `EDIT_READ_ONLY_FIELD` — read-only (edit form)
- `SHOW_FIELD_PREVIEW` — show the field when generating a link to the list item
  
To enable a setting, use `Y`, to disable it — `N`. ||
|| **USER_TYPE_SETTINGS**
[`array`](../../data-types.md) | Array of settings for custom fields. The structure depends on the field type:

- HTML/text — array in the format `{'height': 200}`, where `height` — height of the editor window	
- Element binding in list form — array in the format `{'size': 1, 'width': 0, 'group': 'N', 'multiple': 'N'}`, where `size` — height of the list, `width` — width limit (`0` - no limit), `group` — grouping by sections, `multiple` — display as a multiple choice list
- Counter — array in the format `{'write': 'N', 'VALUE': 1}`, where `write` — permission to change values, `VALUE` — current counter value
- CRM element binding — array in the format `{'VISIBLE': 'N', 'LEAD': 'N', 'CONTACT': 'N', 'COMPANY': 'N', 'DEAL': 'N'}`, where `VISIBLE` — visibility in the CRM card, `LEAD` — lead, `CONTACT` — contact, `COMPANY` — company, `DEAL` — deal. ||
|| **ROW_COUNT**
[`integer`](../../data-types.md) | Height of the field. ||
|| **COL_COUNT**
[`integer`](../../data-types.md) | Width of the field. ||
|| **LINK_IBLOCK_ID**
[`integer`](../../data-types.md) | Identifier of the related list. Required for types Binding to sections, Binding to elements, and Binding to elements in list form. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"123","FIELD_ID":"PROPERTY_1151","FIELDS":{"NAME":"Task Status","SORT":"50","IS_REQUIRED":"N","MULTIPLE":"N","TYPE":"L","LIST":{"1669":{"VALUE":"Planning","SORT":10},"1671":{"VALUE":"In Progress","SORT":20},"1673":{"VALUE":"Testing","SORT":30},"1675":{"VALUE":"Completed","SORT":40},"1677":{"VALUE":"Deferred","SORT":50}},"LIST_TEXT_VALUES":"Archive","LIST_DEF":["1671"],"SETTINGS":{"SHOW_ADD_FORM":"Y","SHOW_EDIT_FORM":"Y","ADD_READ_ONLY_FIELD":"N","EDIT_READ_ONLY_FIELD":"Y","SHOW_FIELD_PREVIEW":"N"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.field.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"123","FIELD_ID":"PROPERTY_1151","FIELDS":{"NAME":"Task Status","SORT":"50","IS_REQUIRED":"N","MULTIPLE":"N","TYPE":"L","LIST":{"1669":{"VALUE":"Planning","SORT":10},"1671":{"VALUE":"In Progress","SORT":20},"1673":{"VALUE":"Testing","SORT":30},"1675":{"VALUE":"Completed","SORT":40},"1677":{"VALUE":"Deferred","SORT":50}},"LIST_TEXT_VALUES":"Archive","LIST_DEF":["1671"],"SETTINGS":{"SHOW_ADD_FORM":"Y","SHOW_EDIT_FORM":"Y","ADD_READ_ONLY_FIELD":"N","EDIT_READ_ONLY_FIELD":"Y","SHOW_FIELD_PREVIEW":"N"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.field.update
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'lists.field.update',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: '123',
                FIELD_ID: 'PROPERTY_1151',
                FIELDS: {
                    NAME: 'Task Status',
                    SORT: '50',
                    IS_REQUIRED: 'N',
                    MULTIPLE: 'N',
                    TYPE: 'L',
                    LIST: {
                        '1669': { VALUE: 'Planning', SORT: 10 },
                        '1671': { VALUE: 'In Progress', SORT: 20 },
                        '1673': { VALUE: 'Testing', SORT: 30 },
                        '1675': { VALUE: 'Completed', SORT: 40 },
                        '1677': { VALUE: 'Deferred', SORT: 50 }
                    },
                    LIST_TEXT_VALUES: 'Archive',
                    LIST_DEF: ['1671'],
                    SETTINGS: {
                        SHOW_ADD_FORM: 'Y',
                        SHOW_EDIT_FORM: 'Y',
                        ADD_READ_ONLY_FIELD: 'N',
                        EDIT_READ_ONLY_FIELD: 'Y',
                        SHOW_FIELD_PREVIEW: 'N'
                    }
                }
            }
        );

        const result = response.getData().result;
        console.log('Updated field:', result);
        processResult(result);
    } catch (error) {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'lists.field.update',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => '123',
                    'FIELD_ID' => 'PROPERTY_1151',
                    'FIELDS' => [
                        'NAME' => 'Task Status',
                        'SORT' => '50',
                        'IS_REQUIRED' => 'N',
                        'MULTIPLE' => 'N',
                        'TYPE' => 'L',
                        'LIST' => [
                            '1669' => ['VALUE' => 'Planning', 'SORT' => 10],
                            '1671' => ['VALUE' => 'In Progress', 'SORT' => 20],
                            '1673' => ['VALUE' => 'Testing', 'SORT' => 30],
                            '1675' => ['VALUE' => 'Completed', 'SORT' => 40],
                            '1677' => ['VALUE' => 'Deferred', 'SORT' => 50]
                        ],
                        'LIST_TEXT_VALUES' => 'Archive',
                        'LIST_DEF' => ['1671'],
                        'SETTINGS' => [
                            'SHOW_ADD_FORM' => 'Y',
                            'SHOW_EDIT_FORM' => 'Y',
                            'ADD_READ_ONLY_FIELD' => 'N',
                            'EDIT_READ_ONLY_FIELD' => 'Y',
                            'SHOW_FIELD_PREVIEW' => 'N'
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.field.update',
        {
            'IBLOCK_TYPE_ID': 'lists',
            'IBLOCK_ID': '123',
            'FIELD_ID': 'PROPERTY_1151',
            'FIELDS': {
                'NAME': 'Task Status',
                'SORT': '50',
                'IS_REQUIRED': 'N',
                'MULTIPLE': 'N',
                'TYPE': 'L', // The type cannot be changed; use the one that exists
                'LIST': {
                    '1669': { 'VALUE': 'Planning', 'SORT': 10 },
                    '1671': { 'VALUE': 'In Progress', 'SORT': 20 },
                    '1673': { 'VALUE': 'Testing', 'SORT': 30 },
                    '1675': { 'VALUE': 'Completed', 'SORT': 40 },
                    '1677': { 'VALUE': 'Deferred', 'SORT': 50 }
                },
                'LIST_TEXT_VALUES': 'Archive',
                'LIST_DEF': ['1671'],
                'SETTINGS': {
                    'SHOW_ADD_FORM': 'Y',
                    'SHOW_EDIT_FORM': 'Y',
                    'ADD_READ_ONLY_FIELD': 'N',
                    'EDIT_READ_ONLY_FIELD': 'Y',
                    'SHOW_FIELD_PREVIEW': 'N'
                }
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.field.update',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => '123',
            'FIELD_ID' => 'PROPERTY_1151',
            'FIELDS' => [
                'NAME' => 'Task Status',
                'SORT' => '50',
                'IS_REQUIRED' => 'N',
                'MULTIPLE' => 'N',
                'TYPE' => 'L',
                'LIST' => [
                    '1669' => ['VALUE' => 'Planning', 'SORT' => 10],
                    '1671' => ['VALUE' => 'In Progress', 'SORT' => 20],
                    '1673' => ['VALUE' => 'Testing', 'SORT' => 30],
                    '1675' => ['VALUE' => 'Completed', 'SORT' => 40],
                    '1677' => ['VALUE' => 'Deferred', 'SORT' => 50]
                ],
                'LIST_TEXT_VALUES' => 'Archive',
                'LIST_DEF' => ['1671'],
                'SETTINGS' => [
                    'SHOW_ADD_FORM' => 'Y',
                    'SHOW_EDIT_FORM' => 'Y',
                    'ADD_READ_ONLY_FIELD' => 'N',
                    'EDIT_READ_ONLY_FIELD' => 'Y',
                    'SHOW_FIELD_PREVIEW' => 'N'
                ]
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
    "result": true,
    "time": {
        "start": 1765371241,
        "finish": 1765371241.084551,
        "duration": 0.08455109596252441,
        "processing": 0,
        "date_start": "2025-12-09T16:54:01+01:00",
        "date_finish": "2025-12-09T16:54:01+01:00",
        "operating_reset_at": 1765371841,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the field was successfully updated. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_UPDATE_FIELD",
    "error_description":"Error updating the field."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not provided. ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | Information block not found. ||
|| `ERROR_UPDATE_FIELD` | Error updating the field | Error updating the field. ||
|| `ERROR_UPDATE_FIELD` | The default value of the field '...' is required | The `ADD_READ_ONLY_FIELD` setting is enabled, but `DEFAULT_VALUE` is not specified. ||
|| `ERROR_UPDATE_FIELD` | Incorrect lists specified for '...' property | Error in one of the parameters inside `FIELDS`. ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to update the field. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-field-add.md)
- [{#T}](./lists-field-get.md)
- [{#T}](./lists-field-delete.md)
- [{#T}](./lists-field-type-get.md)