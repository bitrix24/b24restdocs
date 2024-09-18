# Get Epic Fields by Its Identifier tasks.api.scrum.epic.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- The structure of the parameter files relates to the Disk module, so it is not described here. A link should be made when the structure description appears in the documentation.

{% endnote %}

{% endif %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

The method retrieves the values of the epic fields by its identifier `id`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **epicId***
[`integer`](../../../data-types.md) | The identifier of the epic.

You can obtain epic identifiers using the method [`tasks.api.scrum.epic.list`](./tasks-api-scrum-epic-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "fields": {
      "id": "1"
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "fields": {
      "id": "1"
    },
    auth=YOUR_ACCESS_TOKEN
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.epic.get
    ```

- JS

    ```js
    const epicId = 1;
    BX24.callMethod(
        'tasks.api.scrum.epic.get',
        {
            id: epicId,
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing a request to the REST API
    $result = CRest::call(
    'tasks.api.scrum.epic.get',
    [
        'fields' => [
            'id' => 1,
        ]
    ]
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    }
    else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "id": 1,
    "groupId": 143,
    "name": "epic",
    "description": "",
    "createdBy": 1,
    "modifiedBy": 0,
    "color": "#69dafc",
    "files": {
        "ID": "136",
        "ENTITY_ID": "TASKS_SCRUM_EPIC",
        "FIELD_NAME": "UF_SCRUM_EPIC_FILES",
        "USER_TYPE_ID": "disk_file",
        "XML_ID": null,
        "SORT": "100",
        "MULTIPLE": "Y",
        "MANDATORY": "N",
        "SHOW_FILTER": "N",
        "SHOW_IN_LIST": "N",
        "EDIT_IN_LIST": "N",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "IBLOCK_ID": null,
            "SECTION_ID": null,
            "UF_TO_SAVE_ALLOW_EDIT": false
        },
        "USER_TYPE": {
            "USER_TYPE_ID": "disk_file",
            "CLASS_NAME": "Bitrix\\Disk\\Uf\\FileUserType",
            "DESCRIPTION": "File (Disk)",
            "BASE_TYPE": "int",
            "TAG": [
                "DISK FILE ID",
                "DOCUMENT ID"
            ]
        },
        "VALUE": [],
        "ENTITY_VALUE_ID": 1,
        "CUSTOM_DATA": {
            "PHOTO_TEMPLATE": ""
        },
        "EDIT_FORM_LABEL": "UF_SCRUM_EPIC_FILES",
        "TAG": "DOCUMENT ID"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | The identifier of the epic ||
|| **groupId**
[`integer`](../../../data-types.md) | The identifier of the group (scrum) to which the epic is attached ||
|| **name**
[`string`](../../../data-types.md) | The name of the epic ||
|| **description**
[`string`](../../../data-types.md) | The description of the epic ||
|| **createdBy**
[`integer`](../../../data-types.md) | The identifier of the user who created the epic ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | The identifier of the user who last modified the epic ||
|| **color**
[`string`](../../../data-types.md) | The color of the epic in HEX format ||
|| **files**
[`object`](../../../data-types.md) | An object containing data about all files attached to the epic ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**  | **Value** ||
|| `0` | Access denied | No access to view epic data ||
|| `0` | Epic not found | The epic does not exist ||
|| `100` | Could not find value for parameter {id} | Incorrect parameter name or parameter not set ||
|| `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | Invalid parameter type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-add.md)
- [{#T}](./tasks-api-scrum-epic-update.md)
- [{#T}](./tasks-api-scrum-epic-list.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)
- [{#T}](./tasks-api-scrum-epic-get-fields.md)