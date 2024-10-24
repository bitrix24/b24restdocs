# Methods for Working with Universal Lists

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Attention!" %}

If the list is populated via the API, you need to consider the property **Binding to CRM entity**, as well as the allowed bindings for it. If the field settings allow binding only to one type of entity, then the field value must be filled with the identifier of that entity **without a prefix**.

If you specify it with a prefix, it will be visually displayed, but it will not be exported to Excel.

{% endnote %}

#|
|| **Method** | **Description** | **Version** ||
|| [lists.add](./lists-add.md) | This method creates a list. | ||
|| [lists.delete](./lists-delete.md) | This method deletes a list. | ||
|| [lists.get](./lists-get.md) | This method returns data on lists. | ||
|| [lists.update](./lists-update.md) | This method updates an existing list. | ||
|| [lists.get.iblock.type.id](./lists-get-iblock-type-id.md) | This method returns the id of the information block type. | ||
|#