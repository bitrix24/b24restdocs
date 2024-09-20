# Custom Fields for Inventory Documents

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- add a link to [`userfieldconfig.*`](.)
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

To create custom fields for inventory documents, standard methods from the class [`userfieldconfig.*`](.) are used.

You can retrieve the list of values for custom fields of inventory documents and update the information in them using the following methods:

#|
|| **Method** | **Description** ||
|| [catalog.userfield.document.list](./catalog-userfield-document-list.md) | Returns a list of values for custom fields of inventory documents. ||
|| [catalog.userfield.document.update](./catalog-userfield-document-update.md) | Updates the values for custom fields of inventory documents. ||
|#