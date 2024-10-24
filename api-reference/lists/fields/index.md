# List of Methods for Working with Universal List Fields

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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
|| [lists.field.add](./lists-field-add.md) | This method creates a list field. | ||
|| [lists.field.delete](./lists-field-delete.md) | This method deletes a list field. | ||
|| [lists.field.get](./lists-field-get.md) | This method returns the field data. | ||
|| [lists.field.type.get](./lists-field-type-get.md) | This method returns the available field types for the specified list. | ||
|| [lists.field.update](./lists-field-update.md) | This method updates a list field. | ||