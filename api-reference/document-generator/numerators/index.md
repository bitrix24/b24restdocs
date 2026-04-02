# Enumerators: Overview of Methods

The methods in this section allow you to create, modify, retrieve, and delete document enumerators.

An enumerator defines the rule for generating document numbers:
- number template, for example, `DG-{NUMBER}` or `INV-{NUMBER}`
- parameters for generating the number in the placeholder `{NUMBER}`: starting value, step, and additional settings for the generator

The created enumerator is used in document templates via the `numeratorId` field of the [documentgenerator.template.add](../templates/document-generator-template-add.md) method.

Features of the REST methods documentgenerator.numerator.*:
- The [documentgenerator.numerator.list](./document-generator-numerator-list.md) method returns all enumerators of the document generator type available in the current Bitrix24
- The [documentgenerator.numerator.update](./document-generator-numerator-update.md) and [documentgenerator.numerator.delete](./document-generator-numerator-delete.md) methods only work for enumerators created through [documentgenerator.numerator.add](./document-generator-numerator-add.md)

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with permission to modify document generator templates

#|
|| [documentgenerator.numerator.add](./document-generator-numerator-add.md) | Adds an enumerator ||
|| [documentgenerator.numerator.update](./document-generator-numerator-update.md) | Modifies an enumerator ||
|| [documentgenerator.numerator.get](./document-generator-numerator-get.md) | Retrieves an enumerator by its identifier ||
|| [documentgenerator.numerator.list](./document-generator-numerator-list.md) | Gets a list of enumerators ||
|| [documentgenerator.numerator.delete](./document-generator-numerator-delete.md) | Deletes an enumerator ||
|#