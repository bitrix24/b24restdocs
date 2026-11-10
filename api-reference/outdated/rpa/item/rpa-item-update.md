# Update Process Element rpa.item.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: a user allowed to edit stage items or change the item stage, or the element author allowed to start the workflow

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative functionality.

{% endnote %}

This method updates the element with the identifier `id` in the process with the identifier `typeId`.

The method checks permissions in the following order.

1. The user must be able to view the element. All Bitrix24 employees can view elements; they are not available to extranet users. Without view access, the method returns an error and does not return the element data. The error code depends on the request:
    - `RPA_ITEM_MOVE_ACCESS_DENIED` — the request contains `stageId`
    - `RPA_ITEM_MODIFY_ACCESS_DENIED` — the request contains custom fields
    - `RPA_ITEM_VIEW_ACCESS_DENIED` — in all other cases

2. If the request contains `stageId`, the method checks the permission to change the stage. The user has it if any of the following is true:
    - the user is listed in the "Users allowed to change item stage" setting of the current stage
    - the user is the element author and is listed in "Users allowed to start the workflow"
    - the user is the element author, and the element is at the first stage

3. If the request changes custom field values, the method checks the permission to edit the element. The user has it if any of the following is true:
    - the user is listed in the "Users allowed to edit stage items" setting of the current stage
    - the user is the element author and is listed in "Users allowed to start the workflow". In this case, the user can edit their own element at any stage

A Bitrix24 administrator passes all checks. The permission to start the workflow does not allow editing or moving other users' elements.

If the request does not change field values, for example, it passes the current values, the edit permission is not checked.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`integer`](../../../data-types.md) | Process identifier ||
|| **id** 
[`integer`](../../../data-types.md) | Element identifier ||
|| **fields*** 
[`object`](../../../data-types.md) | Object containing values for custom [fields](#fields) of the element ||
|#

## Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **stageId** 
[`integer`](../../../data-types.md) | Stage identifier ||
|| **UF_RPA_...** 
[`any`](../../../data-types.md) | Values for custom fields. The format for passing values depends on the field type ||
|#

## Code Examples

1. Upload a new file to replace the old one (non-multiple field)

    To replace a file in a non-multiple field, simply upload the new file. The old one will be automatically deleted.

    {% list tabs %}

    - JS

        ```json
        {
            "fields": {
                "UF_RPA_1_1585069397": [
                    "myfile.pdf", "...base64_encoded_file_content..."
                ]
            }
        }
        ```

    {% endlist %}

2. Remove the value of a custom file field

    To do this, simply pass an empty string `''` instead of the value.

3. Keep the value of a non-multiple file field unchanged

    The simplest option is to not include the key for this field in `fields`. However, if you need to pass it without changing it, you should provide a list where the key `id` contains the file identifier.

    {% list tabs %}

    - JS

        ```json
        {
            "fields": {
                "UF_RPA_1_1585069397": {
                    "id": 433    
                }
            }
        }
        ```

    {% endlist %}

    {% note warning %}

    If a value different from the current one is passed in `id`, the field value will be reset and the file will be deleted.

    {% endnote %}

4. Working with a multiple file field

    The value of a multiple field is an array. Each element of the array follows the same rules as for non-multiple values.

5. Partial overwrite of a multiple file field value

    For example, currently the multiple file field contains the values `[12, 255, 44]`.

    You need to keep files `12` and `44`, and upload a new file instead of `255`.

    The request should look as follows:

    {% list tabs %}

    - JS

        ```json
        {
            "fields": {
                "UF_RPA_1_1585069397": [
                    {
                        "id": 12
                    },
                    {
                        "id": 44
                    },
                    [
                        "myNewFile.pdf",
                        "...base64_encoded_file_content..."
                    ]
                ]
            }
        }
        ```

    {% endlist %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "RPA_ITEM_MODIFY_ACCESS_DENIED",
    "error_description": "You do not have permission to edit items in stage Approval"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `RPA_ITEM_VIEW_ACCESS_DENIED` | You don't have permission to view this item | The user cannot view the element, and the request contains neither `stageId` nor custom fields ||
|| `400` | `RPA_ITEM_MOVE_ACCESS_DENIED` | You do not have permission to change item stage from stage #STAGE# | No permission to change the element stage. If the user cannot view the element, the error description is "You don't have permission to view this item" ||
|| `400` | `RPA_ITEM_MODIFY_ACCESS_DENIED` | You do not have permission to edit items in stage #STAGE# | No permission to edit the element. If the user cannot view the element, the error description is "You don't have permission to view this item" ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-item-add.md)
- [{#T}](./rpa-item-get.md)
- [{#T}](./rpa-item-get-tasks.md)
- [{#T}](./rpa-item-list.md)
- [{#T}](./rpa-item-delete.md)