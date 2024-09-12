# Create a New Workflow Element rpa.item.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing fields table
- missing examples
- missing error response

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.item.add` creates a new workflow element with the identifier typeId.

To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

#|
|| **Parameter** / **Type** | **Description** ||
|| **typeId**^*^
[`number`](../../../data-types.md) | Identifier of the workflow.||
|| **fields**
[`array`](../../../data-types.md) | Values of the custom fields of the element. All other fields will be ignored. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

{% note warning %}

After creating the element, automation rules will be triggered automatically.

{% endnote %}

## Success Response

The method will return a result similar to the call of the method [`rpa.item.get`](./rpa-item-get.md) on the newly created element.