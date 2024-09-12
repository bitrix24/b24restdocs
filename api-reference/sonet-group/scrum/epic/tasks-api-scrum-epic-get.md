# Get Epic Fields by Its Identifier tasks.api.scrum.epic.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing
 
{% endnote %}

{% endif %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

The method `tasks.api.scrum.epic.get` returns the values of the fields of an epic by its identifier.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **epicId^*^**
[`integer`](../../../data-types.md) | Epic identifier. ||
|#

## Examples

{% list tabs %}

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

- cURL (oAuth)

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
    } else {
    print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
  "id": 1,
  "groupId": 143,
  "name": "epic",
  "description": "",
  "createdBy": 1,
  "modifiedBy": 0,
  "color": "#69dafc",
  "files":
  {
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
    "SETTINGS":
    {
      "IBLOCK_ID": null,
      "SECTION_ID": null,
      "UF_TO_SAVE_ALLOW_EDIT": false
    },
    "USER_TYPE":
    {
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
    "CUSTOM_DATA":
    {
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
[`integer`](../../../data-types.md) | Epic identifier ||
|| **groupId**
[`integer`](../../../data-types.md) | Group identifier (scrum) to which the epic is attached ||
|| **name**
[`string`](../../../data-types.md) | Epic name ||
|| **description**
[`string`](../../../data-types.md) | Epic description ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the epic ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the epic ||
|| **color**
[`string`](../../../data-types.md) | Epic color in HEX format ||
|| **files**
[`object`](../../../data-types.md) | Object containing data about all files attached to the sprint ||

|#
{% include [Example Footnote](../../../../_includes/examples.md) %}

## Error Handling

HTTP status: **200**

```json
{
  "error": 0,
  "error_description": "Epic not found"
}
```

### Possible Error Codes

#|
|| **Code** | **Description**  | **Value** ||
|| `0` | Access denied | No access to view epic data ||
|| `0` | Epic not found | The epic does not exist ||
|| `100` | Could not find value for parameter {id} | Incorrect parameter name or parameter not set ||
|| `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | Invalid parameter type ||
|#

{% include [Example Footnote](../../../../_includes/examples.md) %}