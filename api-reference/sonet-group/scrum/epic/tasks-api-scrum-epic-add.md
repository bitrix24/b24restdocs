# Add Epic in Scrum tasks.api.scrum.epic.add

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

This method adds an epic to Scrum.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for adding a new epic in the form of a structure:

```js
fields: {
    name: 'value',
    groupId: 'value',
    description: 'value',
    color: 'value',
    files: [
        'file1',
        'file2',
        ...
    ]

}
```

||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Epic name ||
|| **description**
[`string`](../../../data-types.md) | Epic description ||
|| **groupId***
[`integer`](../../../data-types.md) | Identifier of the group (scrum) to which the epic belongs ||
|| **color**
[`string`](../../../data-types.md) | Epic color ||
|| **files**
[`array`](../../../data-types.md) | Array of files associated with the epic.

In `files`, you can pass an array of values with file identifiers, specifying the prefix `n` for each identifier ||
|| **createdBy**
[`integer`](../../../data-types.md) | Created by ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Modified by ||
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
        "name": "Epic 1",
        "groupId": 1,
        "description": "Description text",
        "color": "#69dafc",
        "files": ["n428", "n345"]
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.add
    ```

- cURL (OAuth)

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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.api.scrum.epic.add',
    		{
    			fields: {
    				name: name,
    				groupId: groupId,
    				description: description,
    				color: color,
    				files: files
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.epic.add',
                [
                    'fields' => [
                        'name'        => $name,
                        'groupId'     => $groupId,
                        'description' => $description,
                        'color'       => $color,
                        'files'       => $files,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding epic: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

    ```php
    require_once('crest.php'); // include CRest PHP SDK

    $groupId = 1;
    $name = 'Epic 1';
    $description = 'Description text';
    $color = '#69dafc';
    $files = ['n428', 'n345'];

    // execute request to REST API
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
        }
    );

    // Process response from Bitrix24
    if (isset($result['error'])) {
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
    "id": 4,
    "groupId": 1,
    "name": "Epic 1",
    "description": "Description text",
    "createdBy": 1,
    "modifiedBy": 1,
    "color": "#69dafc"
}
```

### Returned Data {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Epic identifier ||
|| **groupId**
[`integer`](../../../data-types.md) | Identifier of the group (scrum) to which the epic is linked ||
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
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Group is not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**  | **Value** ||
|| `0` | Access denied | No access to scrum ||
|| `0` | Epic not created | Failed to create epic ||
|| `0` | createdBy user not found | User in the "creator" field not found ||
|| `0` | modifiedBy user not found | User in the "last modified by" field not found ||
|| `0` | Group is not found | `GROUP_ID` parameter not specified or group with such `ID` does not exist ||
|| `0` | Name is not found | `NAME` parameter not specified ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-update.md)
- [{#T}](./tasks-api-scrum-epic-get.md)
- [{#T}](./tasks-api-scrum-epic-list.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)
- [{#T}](./tasks-api-scrum-epic-get-fields.md)