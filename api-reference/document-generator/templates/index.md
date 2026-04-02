# Templates in the Document Generator

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the methods: users with permission to modify document generator templates

The methods in this section allow you to create, modify, retrieve, and delete templates, as well as obtain the detail form of template fields.

Features of the REST methods `documentgenerator.template.*`:
- Templates are created in the `rest` module and are linked to a single provider `\Bitrix\DocumentGenerator\DataProvider\Rest`.
- When creating, `moduleId` and `providers` are automatically populated.
- By default, when retrieving the list, only non-deleted templates are returned (`isDeleted = "N"`).
- Since the interface must be implemented independently, visibility settings (parameter `fields[users]`) are only needed by the application itself. Similarly, the sort index (parameter `fields[sort]`) and activity (parameter `fields[active]`) are also required.

#|
|| **Method** | **Description** ||
|| [documentgenerator.template.add](./document-generator-template-add.md) | Adds a new document template ||
|| [documentgenerator.template.update](./document-generator-template-update.md) | Updates an existing template ||
|| [documentgenerator.template.get](./document-generator-template-get.md) | Returns a template by its identifier ||
|| [documentgenerator.template.list](./document-generator-template-list.md) | Returns a list of templates based on a filter ||
|| [documentgenerator.template.delete](./document-generator-template-delete.md) | Deletes a template ||
|| [documentgenerator.template.getfields](./document-generator-template-get-fields.md) | Returns the fields of the template and their current values ||
|#