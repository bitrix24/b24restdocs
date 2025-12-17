# Create a Field for Universal List lists.field.add

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Full access" permission for the required list

The method `lists.field.add` creates a list field.

{% note info "" %}

You can find the available field types for the universal list using the method [lists.field.type.get](./lists-field-type-get.md)

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
  
The identifier can be obtained using the method [lists.get.iblock.type.id](../lists/lists-get-iblock-type-id.md) ||
|| **IBLOCK_ID***
[`integer`](../../data-types.md) | Identifier of the information block.

The identifier can be obtained using the method [lists.get](../lists/lists-get.md) ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the method [lists.get](../lists/lists-get.md)

{% note info "" %}

At least one of the parameters: `IBLOCK_ID` or `IBLOCK_CODE` must be specified.

{% endnote %} ||
|| **FIELDS***
[`array`](../../data-types.md) | Array of parameters.

[Detailed description](#parametr-fields) ||
|#

### Parameter FIELDS {#parametr-fields}

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../data-types.md) | Field name ||
|| **TYPE***
[`string`](../../data-types.md) | Field type. Once created, the field type cannot be changed.

Custom fields:
- `S` — String
- `N` — Number
- `L` — List
- `F` — File
- `G` — Section binding
- `E` — Element binding
- `S:Date` — Date
- `S:DateTime` — Date/Time
- `S:HTML` — HTML/Text
- `E:EList` — Element binding as a list
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
  
Values in the fields Creation date, Modification date, Created by, and Modified by are filled automatically.

{% note info "" %}

System fields are not added to the new list by default. To display them in the list interface, they must also be explicitly created.

{% endnote %}
||
|| **IS_REQUIRED**
[`string`](../../data-types.md) | Field required flag. Possible values:
- `Y` — yes
- `N` — no
  
Default — `N` ||
|| **MULTIPLE**
[`string`](../../data-types.md) | Field multiplicity flag. Possible values:
- `Y` — yes
- `N` — no  

Default — `N`.

System fields cannot be multiple ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting ||
|| **DEFAULT_VALUE** | Default value. If the `ADD_READ_ONLY_FIELD` setting is enabled in `SETTINGS`, this parameter becomes required ||
|| **LIST**
[`array`](../../data-types.md) | Values for the List type field. An array in the format `{'ID': { 'VALUE': 'Value_1', 'SORT': 10, 'DEF': 'N' }}`, where `'ID'` — temporary identifier, `VALUE` — displayed text, `SORT` — sorting order, `DEF` — default value indicator.

After creating the field, the system will replace temporary identifiers with permanent ones ||
|| **LIST_TEXT_VALUES**
[`string`](../../data-types.md) | An alternative way to specify values for the List type field. A string where values are separated by the newline character `\n`

{% note info "" %}

When creating a List type field, you can use either `LIST`, `LIST_TEXT_VALUES`, or both parameters together. In this case, the values from the string will be added to those already present in the array.

{% endnote %} ||
|| **LIST_DEF**
[`array`](../../data-types.md) | Default value for the List type field. The array accepts a temporary identifier from `LIST` ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the field. Required for custom fields. Not used for system fields ||
|| **SETTINGS**
[`array`](../../data-types.md) | Display and behavior settings.

Supported values:
- `SHOW_ADD_FORM` — show in the add form
- `SHOW_EDIT_FORM` — show in the edit form
- `ADD_READ_ONLY_FIELD` — read-only (add form)
- `EDIT_READ_ONLY_FIELD` — read-only (edit form)
- `SHOW_FIELD_PREVIEW` — show field when generating a link to the list item
  
To enable a setting, use `Y`, to disable — `N`.

Default: `SHOW_ADD_FORM` — `Y`, `SHOW_EDIT_FORM` — `Y`, `ADD_READ_ONLY_FIELD` — `N`, `EDIT_READ_ONLY_FIELD` — `N`, `SHOW_FIELD_PREVIEW` — `N` ||
|| **USER_TYPE_SETTINGS**
[`array`](../../data-types.md) | Array of settings for custom fields. The structure depends on the field type:

- HTML/Text — array in the format `{'height': 200}`, where `height` — height of the editor window	
- Element binding as a list — array in the format `{'size': 1, 'width': 0, 'group': 'N', 'multiple': 'N'}`, where `size` — height of the list, `width` — width restriction (`0` - no restriction), `group` — grouping by sections, `multiple` — display as a multiple choice list
- Counter — array in the format `{'write': 'N', 'VALUE': 1}`, where `write` — permission to change values, `VALUE` — current counter value
- CRM element binding — array in the format `{'VISIBLE': 'N', 'LEAD': 'N', 'CONTACT': 'N', 'COMPANY': 'N', 'DEAL': 'N'}`, where `VISIBLE` — visibility in the CRM card, `LEAD` — lead, `CONTACT` — contact, `COMPANY` — company, `DEAL` — deal 

If not passed — the system will set default values as specified in the array examples ||
|| **ROW_COUNT**
[`integer`](../../data-types.md) | Height of the field.

Default — `1` ||
|| **COL_COUNT**
[`integer`](../../data-types.md) | Width of the field.

Default — `30` ||
|| **LINK_IBLOCK_ID**
[`integer`](../../data-types.md) | Identifier of the related list. Required for types Binding to sections, Binding to elements, and Binding to elements as a list ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"123","FIELDS":{"NAME":"Project","IS_REQUIRED":"Y","MULTIPLE":"N","TYPE":"L","SORT":"10","CODE":"PROJECT","LIST":{"10":{"VALUE":"Planning","SORT":10,"DEF":"Y"},"20":{"VALUE":"In Development","SORT":20,"DEF":"N"}},"LIST_TEXT_VALUES":"Testing\nCompleted\nDeferred","SETTINGS":{"SHOW_ADD_FORM":"Y","SHOW_EDIT_FORM":"Y","ADD_READ_ONLY_FIELD":"N","EDIT_READ_ONLY_FIELD":"N","SHOW_FIELD_PREVIEW":"N"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.field.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"123","FIELDS":{"NAME":"Project","IS_REQUIRED":"Y","MULTIPLE":"N","TYPE":"L","SORT":"10","CODE":"PROJECT","LIST":{"10":{"VALUE":"Planning","SORT":10,"DEF":"Y"},"20":{"VALUE":"In Development","SORT":20,"DEF":"N"}},"LIST_TEXT_VALUES":"Testing\nCompleted\nDeferred","SETTINGS":{"SHOW_ADD_FORM":"Y","SHOW_EDIT_FORM":"Y","ADD_READ_ONLY_FIELD":"N","EDIT_READ_ONLY_FIELD":"N","SHOW_FIELD_PREVIEW":"N"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.field.add
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'lists.field.add',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: '123',
                FIELDS: {
                    NAME: 'Project',
                    IS_REQUIRED: 'Y',
                    MULTIPLE: 'N',
                    TYPE: 'L',
                    SORT: '10',
                    CODE: 'PROJECT',
                    LIST: {
                        '10': { VALUE: 'Planning', SORT: 10, DEF: 'Y' },
                        '20': { VALUE: 'In Development', SORT: 20, DEF: 'N' }
                    },
                    LIST_TEXT_VALUES: 'Testing\nCompleted\nDeferred',
                    SETTINGS: {
                        SHOW_ADD_FORM: 'Y',
                        SHOW_EDIT_FORM: 'Y',
                        ADD_READ_ONLY_FIELD: 'N',
                        EDIT_READ_ONLY_FIELD: 'N',
                        SHOW_FIELD_PREVIEW: 'N'
                    }
                }
            }
        );

        const result = response.getData().result;
        console.log('Created field with ID:', result);
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
                'lists.field.add',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => '123',
                    'FIELDS' => [
                        'NAME' => 'Project',
                        'IS_REQUIRED' => 'Y',
                        'MULTIPLE' => 'N',
                        'TYPE' => 'L',
                        'SORT' => '10',
                        'CODE' => 'PROJECT',
                        'LIST' => [
                            '10' => ['VALUE' => 'Planning', 'SORT' => 10, 'DEF' => 'Y'],
                            '20' => ['VALUE' => 'In Development', 'SORT' => 20, 'DEF' => 'N']
                        ],
                        'LIST_TEXT_VALUES' => 'Testing\nCompleted\nDeferred',
                        'SETTINGS' => [
                            'SHOW_ADD_FORM' => 'Y',
                            'SHOW_EDIT_FORM' => 'Y',
                            'ADD_READ_ONLY_FIELD' => 'N',
                            'EDIT_READ_ONLY_FIELD' => 'N',
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
        echo 'Error adding field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.field.add',
        {
            'IBLOCK_TYPE_ID': 'lists',
            'IBLOCK_ID': '123',
            'FIELDS': {
                'NAME': 'Project',
                'IS_REQUIRED': 'Y',
                'MULTIPLE': 'N',
                'TYPE': 'L',
                'SORT': '10',
                'CODE': 'PROJECT',
                'LIST': {
                    '10': { 'VALUE': 'Planning', 'SORT': 10, 'DEF': 'Y' },
                    '20': { 'VALUE': 'In Development', 'SORT': 20, 'DEF': 'N' }
                },Э
                'LIST_TEXT_VALUES': 'Testing\nCompleted\nDeferred',
                'SETTINGS': {
                    'SHOW_ADD_FORM': 'Y',
                    'SHOW_EDIT_FORM': 'Y',
                    'ADD_READ_ONLY_FIELD': 'N',
                    'EDIT_READ_ONLY_FIELD': 'N',
                    'SHOW_FIELD_PREVIEW': 'N'
                }
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
                // The final list will contain 5 options:
                // 1. Planning (default), 2. In Development,
                // 3. Testing, 4. Completed, 5. Deferred
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.field.add',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => '123',
            'FIELDS' => [
                'NAME' => 'Project',
                'IS_REQUIRED' => 'Y',
                'MULTIPLE' => 'N',
                'TYPE' => 'L',
                'SORT' => '10',
                'CODE' => 'PROJECT',
                'LIST' => [
                    '10' => ['VALUE' => 'Planning', 'SORT' => 10, 'DEF' => 'Y'],
                    '20' => ['VALUE' => 'In Development', 'SORT' => 20, 'DEF' => 'N']
                ],
                'LIST_TEXT_VALUES' => 'Testing\nCompleted\nDeferred',
                'SETTINGS' => [
                    'SHOW_ADD_FORM' => 'Y',
                    'SHOW_EDIT_FORM' => 'Y',
                    'ADD_READ_ONLY_FIELD' => 'N',
                    'EDIT_READ_ONLY_FIELD' => 'N',
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
    "result": "PROPERTY_1151",
    "time": {
        "start": 1765317940,
        "finish": 1765317940.172892,
        "duration": 0.17289209365844727,
        "processing": 0,
        "date_start": "2025-12-09T14:05:40+01:00",
        "date_finish": "2025-12-09T14:05:40+01:00",
        "operating_reset_at": 1765318540,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../data-types.md) | Identifier of the created field with the prefix `PROPERTY_`.

If a system field is created, its symbolic code is returned ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_SAVE_FIELD",
    "error_description":"Please fill the code fields"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not passed ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | Information block not found ||
|| `ERROR_SAVE_FIELD` | Error saving the field | General error during the call ||
|| `ERROR_SAVE_FIELD` | Property already exists | `CODE` is already in use in the list ||
|| `ERROR_SAVE_FIELD` | Please fill the code fields | `CODE` not specified for custom field ||
|| `ERROR_SAVE_FIELD` | The default value of the field '...' is required | The `ADD_READ_ONLY_FIELD` setting is enabled, but `DEFAULT_VALUE` is not specified ||
|| `ERROR_SAVE_FIELD` | Incorrect lists specified for '...' property | Error in one of the parameters inside `FIELDS` ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to add the field ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-field-update.md)
- [{#T}](./lists-field-get.md)
- [{#T}](./lists-field-delete.md)
- [{#T}](./lists-field-type-get.md)