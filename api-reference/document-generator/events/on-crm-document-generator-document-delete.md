# On Document Deletion onCrmDocumentGeneratorDocumentDelete

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- field types are not specified
- field requirements are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmDocumentGeneratorDocumentDelete` – document deletion.

The event handler will receive data in the following format:

```php
[
    'FIELDS' => [
        'ID' => $documentId,
        'ENTITY_TYPE_ID' => $entityTypeId,
        'ENTITY_ID' => $entityId,
    ],
]
```
#|
|| **Field** | **Description** ||
|| **$documentId** | Document identifier. ||
|| **$entityTypeId** | CRM type identifier. ||
|| **$entityId** | Entity identifier. ||
|#