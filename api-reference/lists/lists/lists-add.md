# Create a universal list lists.add

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `lists.add` method creates a universal list.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_TYPE_ID***
[`string`](../../data-types.md) | Identifier of the information block type. Possible values: 
- `lists` — list information block type 
- `bitrix_processes` — processes information block type 
- `lists_socnet` — group lists information block type ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block ||
|| **SOCNET_GROUP_ID**
[`integer`](../../data-types.md) | Group identifier. Use this parameter if you want to add the list to a group.

The identifier can be obtained using the methods [socialnetwork.api.workgroup.list](../../sonet-group/socialnetwork-api-workgroup-list.md), [sonet_group.get](../../sonet-group/sonet-group-get.md), and [sonet_group.user.groups](../../sonet-group/sonet-group-user-groups.md) ||
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

You can obtain the user identifier using the method [user.get](../../user/user-get.md), and the department identifier using the method [department.get](../../departments/department-get.md).

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

If the parameter is not provided, the authorized user who called the method is assigned full access.

{% endnote %}
||
|#

### Parameter FIELDS {#parametr-fields}

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
[`array`](../../data-types.md) | Picture. An object in the format `{fileData: [value1, value2]}`, where `value1` is the name of the picture file with the extension, and `value2` is the picture in [base64](../../files/how-to-upload-files.md#kak-kodirovat-fajl-v-base64) format ||
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
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_CODE":"my_custom_list","FIELDS":{"NAME":"My new list","DESCRIPTION":"List for tracking tasks in the project","SORT":500,"BIZPROC":"Y"},"MESSAGES":{"ELEMENTS_NAME":"Tasks","ELEMENT_NAME":"Task","ELEMENT_ADD":"Add task","ELEMENT_EDIT":"Edit task","ELEMENT_DELETE":"Delete task","SECTIONS_NAME":"Sections","SECTION_NAME":"Section","SECTION_ADD":"Add section","SECTION_EDIT":"Edit section","SECTION_DELETE":"Delete section"},"RIGHTS":{"U1271":"X"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_CODE":"my_custom_list","FIELDS":{"NAME":"My new list","DESCRIPTION":"List for tracking tasks in the project","SORT":500,"BIZPROC":"Y"},"MESSAGES":{"ELEMENTS_NAME":"Tasks","ELEMENT_NAME":"Task","ELEMENT_ADD":"Add task","ELEMENT_EDIT":"Edit task","ELEMENT_DELETE":"Delete task","SECTIONS_NAME":"Sections","SECTION_NAME":"Section","SECTION_ADD":"Add section","SECTION_EDIT":"Edit section","SECTION_DELETE":"Delete section"},"RIGHTS":{"U1271":"X"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.add
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
          'lists.add',
          {
              IBLOCK_TYPE_ID: 'lists',
              IBLOCK_CODE: 'my_custom_list',
              FIELDS: {
                NAME: 'My new list',
                DESCRIPTION: 'List for tracking tasks in the project',
                SORT: 500,
                BIZPROC: 'Y'
              },
              MESSAGES: {
                ELEMENTS_NAME: 'Tasks',
                ELEMENT_NAME: 'Task',
                ELEMENT_ADD: 'Add task',
                ELEMENT_EDIT: 'Edit task',
                ELEMENT_DELETE: 'Delete task',
                SECTIONS_NAME: 'Sections',
                SECTION_NAME: 'Section',
                SECTION_ADD: 'Add section',
                SECTION_EDIT: 'Edit section',
                SECTION_DELETE: 'Delete section'
              },
              RIGHTS: {
                'U1271': 'X'
              }
          }
      );

        const result = response.getData().result;
        console.log('Created list with ID:', result);
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
                'lists.add',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_CODE' => 'my_custom_list',
                    'FIELDS' => [
                        'NAME' => 'My new list',
                        'DESCRIPTION' => 'List for tracking tasks in the project',
                        'SORT' => 500,
                        'BIZPROC' => 'Y'
                    ],
                    'MESSAGES' => [
                        'ELEMENTS_NAME' => 'Tasks',
                        'ELEMENT_NAME' => 'Task',
                        'ELEMENT_ADD' => 'Add task',
                        'ELEMENT_EDIT' => 'Edit task',
                        'ELEMENT_DELETE' => 'Delete task',
                        'SECTIONS_NAME' => 'Sections',
                        'SECTION_NAME' => 'Section',
                        'SECTION_ADD' => 'Add section',
                        'SECTION_EDIT' => 'Edit section',
                        'SECTION_DELETE' => 'Delete section'
                    ],
                    'RIGHTS' => [
                        'U1271' => 'X'
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
        echo 'Error adding list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'lists.add',
      {
         IBLOCK_TYPE_ID: 'lists',
         IBLOCK_CODE: 'my_custom_list',
         FIELDS: {
           NAME: 'My new list',
           DESCRIPTION: 'List for tracking tasks in the project',
           SORT: 500,
           BIZPROC: 'Y'
         },
         MESSAGES: {
           ELEMENTS_NAME: 'Tasks',
           ELEMENT_NAME: 'Task',
           ELEMENT_ADD: 'Add task',
           ELEMENT_EDIT: 'Edit task',
           ELEMENT_DELETE: 'Delete task',
           SECTIONS_NAME: 'Sections',
           SECTION_NAME: 'Section',
           SECTION_ADD: 'Add section',
           SECTION_EDIT: 'Edit section',
           SECTION_DELETE: 'Delete section'
         },
         RIGHTS: {
           'U1271': 'X',     // user with ID=1271 — full access
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
        'lists.add',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_CODE' => 'my_custom_list',
            'FIELDS' => [
                'NAME' => 'My new list',
                'DESCRIPTION' => 'List for tracking tasks in the project',
                'SORT' => 500,
                'BIZPROC' => 'Y'
            ],
            'MESSAGES' => [
                'ELEMENTS_NAME' => 'Tasks',
                'ELEMENT_NAME' => 'Task',
                'ELEMENT_ADD' => 'Add task',
                'ELEMENT_EDIT' => 'Edit task',
                'ELEMENT_DELETE' => 'Delete task',
                'SECTIONS_NAME' => 'Sections',
                'SECTION_NAME' => 'Section',
                'SECTION_ADD' => 'Add section',
                'SECTION_EDIT' => 'Edit section',
                'SECTION_DELETE' => 'Delete section'
            ],
            'RIGHTS' => [
                'U1271' => 'X'
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
    "result": 109,
    "time": {
       "start": 1764675143,
       "finish": 1764675143.174578,
       "duration": 0.17457795143127441,
       "processing": 0,
       "date_start": "2025-12-02T14:32:23+01:00",
       "date_finish": "2025-12-02T14:32:23+01:00",
       "operating_reset_at": 1764675743,
       "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created information block ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_IBLOCK_ALREADY_EXISTS",
    "error_description":"Iblock already exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter is missing ||
|| `ERROR_IBLOCK_ALREADY_EXISTS` | Iblock already exists | A list with this `IBLOCK_CODE` already exists ||
|| `ERROR_ADD_IBLOCK` | — | Error adding the list ||
|| `ACCESS_DENIED` | Access denied | The method was called by a non-administrator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-update.md)
- [{#T}](./lists-get.md)
- [{#T}](./lists-delete.md)
- [{#T}](./lists-get-iblock-type-id.md)