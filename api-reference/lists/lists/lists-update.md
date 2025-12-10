# Update Universal List lists.update

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Full access" permission for the required list

The `lists.update` method updates the universal list.

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

The identifier can be obtained using the [lists.get-iblock-type-id](./lists-get-iblock-type-id.md) method ||
|| **IBLOCK_ID***
[`integer`](../../data-types.md) | Identifier of the information block.

The identifier can be obtained using the [lists.get](../lists/lists-get.md) method ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the [lists.get](../lists/lists-get.md) method

{% note info "" %}

At least one of the parameters: `IBLOCK_ID` or `IBLOCK_CODE` must be specified.

{% endnote %} ||
|| **SOCNET_GROUP_ID**
[`integer`](../../data-types.md) | Group identifier. It is not necessary to specify this for changing list settings. Use this parameter if you want to move the list to another group.

The identifier can be obtained using the [socialnetwork.api.workgroup.list](../../sonet-group/socialnetwork-api-workgroup-list.md), [sonet_group.get](../../sonet-group/sonet-group-get.md), and [sonet_group.user.groups](../../sonet-group/sonet-group-user-groups.md) methods ||
|| **FIELDS***
[`array`](../../data-types.md) | Array of list fields.

[Detailed description](#parametr-fields) ||
|| **MESSAGES**
[`array`](../../data-types.md) | Array of labels for list items and sections. Supported values:

- `ELEMENTS_NAME` — name of the items
- `ELEMENT_NAME` — name of the item
- `ELEMENT_ADD` — add item
- `ELEMENT_EDIT` — edit item
- `ELEMENT_DELETE` — delete item
- `SECTIONS_NAME` — name of the sections
- `SECTION_NAME` — name of the section
- `SECTION_ADD` — add section
- `SECTION_EDIT` — edit section
- `SECTION_DELETE` — delete section
||
|| **RIGHTS**
[`array`](../../data-types.md) | Access permission settings for the list. An array in key-value format, where the key is the letter code of the user or department with the identifier, and the value is the letter code of the permission. 

```js
RIGHTS: {
    'U1': 'X', // User with ID=1 — full access
    'D5': 'D'  // Employees of the department with ID=5 — access denied
}
```

User categories:
- `U` — user
- `*` — all users
- `D` — all department employees
- `DR` — all department employees with subdivisions

You can obtain the user identifier using the [user.get](../../user/user-get.md) method, and the department identifier using the [department.get](../../departments/department-get.md) method.

Types of permissions:
- `D` — no access
- `R` — read
- `E` — add
- `S` — view in panel
- `T` — add in panel
- `U` — modify with restrictions
- `W` — modify
- `X` — full access

{% note info "" %}

Access permissions can only be assigned to users or departments that do not already have them. Existing permissions cannot be changed.

{% endnote %}
||
|#

### FIELDS Parameter {#parametr-fields}

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../data-types.md) | Name of the list ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the list ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting ||
|| **PICTURE**
[`array`](../../data-types.md) | Picture. An object in the format `{fileData: [value1, value2]}`, where `value1` is the name of the picture file with the extension, and `value2` is the picture in [base64](../../files/how-to-update-files.md#kak-kodirovat-fajl-v-base64) format ||
|| **BIZPROC**
[`string`](../../data-types.md) | Enabling business process support. Possible values:
- `Y` — yes
- `N` — no 
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":109,"FIELDS":{"NAME":"Updated Task List","DESCRIPTION":"Updated description: list for managing daily tasks","SORT":600,"BIZPROC":"N"},"MESSAGES":{"ELEMENTS_NAME":"Items","ELEMENT_NAME":"Item","ELEMENT_ADD":"Create item","ELEMENT_EDIT":"Edit item","ELEMENT_DELETE":"Delete item","SECTIONS_NAME":"Categories","SECTION_NAME":"Category","SECTION_ADD":"Add category","SECTION_EDIT":"Edit category","SECTION_DELETE":"Delete category"},"RIGHTS":{"D15":"W"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":109,"FIELDS":{"NAME":"Updated Task List","DESCRIPTION":"Updated description: list for managing daily tasks","SORT":600,"BIZPROC":"N"},"MESSAGES":{"ELEMENTS_NAME":"Items","ELEMENT_NAME":"Item","ELEMENT_ADD":"Create item","ELEMENT_EDIT":"Edit item","ELEMENT_DELETE":"Delete item","SECTIONS_NAME":"Categories","SECTION_NAME":"Category","SECTION_ADD":"Add category","SECTION_EDIT":"Edit category","SECTION_DELETE":"Delete category"},"RIGHTS":{"D15":"W"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.update
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod(
          'lists.update',
          {
              IBLOCK_TYPE_ID: 'lists',
              IBLOCK_ID: 109,
              FIELDS: {
                NAME: 'Updated Task List',
                DESCRIPTION: 'Updated description: list for managing daily tasks',
                SORT: 600,
                BIZPROC: 'N'
              },
              MESSAGES: {
                ELEMENTS_NAME: 'Items',
                ELEMENT_NAME: 'Item',
                ELEMENT_ADD: 'Create item',
                ELEMENT_EDIT: 'Edit item',
                ELEMENT_DELETE: 'Delete item',
                SECTIONS_NAME: 'Categories',
                SECTION_NAME: 'Category',
                SECTION_ADD: 'Add category',
                SECTION_EDIT: 'Edit category',
                SECTION_DELETE: 'Delete category'
              },
              RIGHTS: {
                'D15': 'W'
              }
          }
      );

      const result = response.getData().result;
      console.log('Updated list with ID:', result);
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
                'lists.update',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => 109,
                    'FIELDS' => [
                        'NAME' => 'Updated Task List',
                        'DESCRIPTION' => 'Updated description: list for managing daily tasks',
                        'SORT' => 600,
                        'BIZPROC' => 'N'
                    ],
                    'MESSAGES' => [
                        'ELEMENTS_NAME' => 'Items',
                        'ELEMENT_NAME' => 'Item',
                        'ELEMENT_ADD' => 'Create item',
                        'ELEMENT_EDIT' => 'Edit item',
                        'ELEMENT_DELETE' => 'Delete item',
                        'SECTIONS_NAME' => 'Categories',
                        'SECTION_NAME' => 'Category',
                        'SECTION_ADD' => 'Add category',
                        'SECTION_EDIT' => 'Edit category',
                        'SECTION_DELETE' => 'Delete category'
                    ],
                    'RIGHTS' => [
                        'D15' => 'W'
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
        echo 'Error updating list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'lists.update',
    {
       IBLOCK_TYPE_ID: 'lists',
       IBLOCK_ID: 109,
       FIELDS: {
         NAME: 'Updated Task List',
         DESCRIPTION: 'Updated description: list for managing daily tasks',
         SORT: 600,
         BIZPROC: 'N' 
       },
       MESSAGES: {
         ELEMENTS_NAME: 'Items',
         ELEMENT_NAME: 'Item',
         ELEMENT_ADD: 'Create item',
         ELEMENT_EDIT: 'Edit item',
         ELEMENT_DELETE: 'Delete item',
         SECTIONS_NAME: 'Categories',
         SECTION_NAME: 'Category',
         SECTION_ADD: 'Add category',
         SECTION_EDIT: 'Edit category',
         SECTION_DELETE: 'Delete category'
       },
       RIGHTS: {
         'D15': 'W',    // employees of department with ID=15 — modification
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
        'lists.update',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 109,
            'FIELDS' => [
                'NAME' => 'Updated Task List',
                'DESCRIPTION' => 'Updated description: list for managing daily tasks',
                'SORT' => 600,
                'BIZPROC' => 'N'
            ],
            'MESSAGES' => [
                'ELEMENTS_NAME' => 'Items',
                'ELEMENT_NAME' => 'Item',
                'ELEMENT_ADD' => 'Create item',
                'ELEMENT_EDIT' => 'Edit item',
                'ELEMENT_DELETE' => 'Delete item',
                'SECTIONS_NAME' => 'Categories',
                'SECTION_NAME' => 'Category',
                'SECTION_ADD' => 'Add category',
                'SECTION_EDIT' => 'Edit category',
                'SECTION_DELETE' => 'Delete category'
            ],
            'RIGHTS' => [
                'D15' => 'W'
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
        "start": 1764687690,
        "finish": 1764687690.350469,
        "duration": 0.35046911239624023,
        "processing": 0,
        "date_start": "2025-12-02T15:01:30+01:00",
        "date_finish": "2025-12-02T15:01:30+01:00",
        "operating_reset_at": 1764688290,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the list was successfully updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_IBLOCK_NOT_FOUND",
    "error_description":"Iblock not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not provided ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | List with such `IBLOCK_ID` or `IBLOCK_CODE` not found ||
|| `ERROR_UPDATE_IBLOCK` | — | Error updating the list ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to update the list ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-add.md)
- [{#T}](./lists-get.md)
- [{#T}](./lists-delete.md)
- [{#T}](./lists-get-iblock-type-id.md)