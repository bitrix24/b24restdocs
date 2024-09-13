# Universal Lists

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

REST methods for working with universal lists.

> Scope: [`lists`](../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Attention!" %}

If the list is populated via the API, you need to consider the property **Binding to CRM entity**, as well as the allowed bindings for it. If the field settings allow binding only to one type of entities, then the field value must be filled with the identifier of that entity **without a prefix**.

If you specify it with a prefix, it will be visually displayed, but it will not be exported to Excel.

{% endnote %}