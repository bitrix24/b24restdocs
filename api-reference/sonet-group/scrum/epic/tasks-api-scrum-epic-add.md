# Add Epic in Scrum tasks.api.scrum.epic.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

The method `tasks.api.scrum.epic.add` adds an epic to Scrum.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **fields^*^**
[`array`](../../../data-types.md) | Fields corresponding to the available list of fields [tasks.api.scrum.epic.getFields](./tasks-api-scrum-epic-get-fields.md).

The fields `name` and `groupId` are required.

In the `files` field, you can pass an array of values with file identifiers, specifying the prefix `n` for each identifier. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    const groupId = 1;
    const name = 'Epic 1';
    const description = 'Description text';
    const color = '#69dafc';
    const files = ['n428', 'n345'];

    BX24.callMethod(
        'tasks.api.scrum.epic.add',
        {
            fields: {
                name: name,
                groupId: groupId,
                description: description,
                color: color,
                files: files
            }
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
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "fields": {
        "name": "Epic 1",
        "groupId": 1,
        "description": "Description text",
        "color": "#69dafc",
        "files": ["n428", "n345"]
    }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.epic.add
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "fields": {
        "name": "Epic 1",
        "groupId": 1,
        "description": "Description text",
        "color": "#69dafc",
        "files": ["n428", "n345"]
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.add
    ```

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $groupId = 1;
    $name = 'Epic 1';
    $description = 'Description text';
    $color = '#69dafc';
    $files = ['n428', 'n345'];

    // executing the request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.epic.add',
        [
            'fields' => [
                'name' => $name,
                'groupId' => $groupId,
                'description' => $description,
                'color' => $color,
                'files' => $files
            ]
        ]
    );

    // Handling the response from Bitrix24
    if (isset($result['error'])) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "id": 4,
    "groupId": 1,
    "name": "Epic 1",
    "description": "Description text",
    "createdBy": 1,
    "modifiedBy": 1,
    "color": "#69dafc"
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the epic ||
|| **groupId**
[`integer`](../../../data-types.md) | Identifier of the group (scrum) to which the epic is attached ||
|| **name**
[`string`](../../../data-types.md) | Name of the epic ||
|| **description**
[`string`](../../../data-types.md) | Description of the epic ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the epic ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the epic ||
|| **color**
[`string`](../../../data-types.md) | Color of the epic in HEX format ||

|#
{% include [Example Notes](../../../../_includes/examples.md) %}

## Error Handling

HTTP Status: **200**

```json
{
  "error": 0,
  "error_description": "Epic not found"
}
```

### Possible Error Codes

#|
|| **Code** | **Description**  | **Value** ||
|| `0` | Access denied | No access to scrum ||
|| `0` | Epic not created | Failed to create epic ||
|| `0` | createdBy user not found | User in the "creator" field not found ||
|| `0` | modifiedBy user not found | User in the "last modified by" field not found ||
|| `0` | Group is not found | GROUP_ID parameter is not specified or a group with such ID does not exist ||
|| `0` | Name is not found | NAME parameter is not specified ||
|#

{% include [Example Notes](../../../../_includes/examples.md) %}