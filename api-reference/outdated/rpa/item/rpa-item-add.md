# Add Process Element rpa.item.add

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative functionality.

{% endnote %}

This method adds a new process element with the identifier `typeId`.

To upload a file, you need to pass an array as the value of the custom field, where the first element is the file name and the second is the base64 encoded content of the file.

After the element is created, Automation rules will be triggered automatically.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **typeId***
[`integer`](../../../data-types.md) | Identifier of the process ||
|| **fields**
[`array`](../../../data-types.md) | Values of the custom fields of the element. All other fields will be ignored ||
|#

## Response Handling

HTTP Status: **200**

The method will return a result similar to the call of the method [`rpa.item.get`](./rpa-item-get.md) on the newly created element.

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-item-update.md)
- [{#T}](./rpa-item-get.md)
- [{#T}](./rpa-item-get-tasks.md)
- [{#T}](./rpa-item-list.md)
- [{#T}](./rpa-item-delete.md)