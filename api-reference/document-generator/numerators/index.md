# Numbering Rules: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Numbering rules define the format for document numbers:
- Number template, for example, `DG-{NUMBER}` or `INV-{NUMBER}`
- Parameters for generating the number in the placeholder `{NUMBER}`: starting value, step, and additional generator settings

> Quick navigation: [all methods](#all-methods)

## Getting Started

1. Retrieve the list of available numbering rules using the [documentgenerator.numerator.list](./document-generator-numerator-list.md) method.
2. If you need a custom numbering rule, create it using the [documentgenerator.numerator.add](./document-generator-numerator-add.md) method.
3. Get the settings of a numbering rule by its identifier using the [documentgenerator.numerator.get](./document-generator-numerator-get.md) method.
4. Pass the numbering rule identifier in the `numeratorId` field [when creating a template](../templates/document-generator-template-add.md) or [updating a template](../templates/document-generator-template-update.md).
5. Modify the parameters of the custom numbering rule using the [documentgenerator.numerator.update](./document-generator-numerator-update.md) method.
6. Delete an unnecessary custom numbering rule using the [documentgenerator.numerator.delete](./document-generator-numerator-delete.md) method.

{% note info " " %}

The [documentgenerator.numerator.list](./document-generator-numerator-list.md) method returns all numbering rules available in the current Bitrix24, including [CRM numbering rules](../../crm/document-generator/numerator/index.md).

{% endnote %}

## Linking Numbering Rules with Other Objects

**Document Templates.** A numbering rule is linked to a template through the `numeratorId` field. To assign a numbering rule to a template, pass the numbering rule identifier to the [documentgenerator.template.add](../templates/document-generator-template-add.md) or [documentgenerator.template.update](../templates/document-generator-template-update.md) methods.

The numbering rule identifier can be obtained after creation or through the [documentgenerator.numerator.list](./document-generator-numerator-list.md) method.

## Considerations When Modifying and Deleting a Numbering Rule

The [documentgenerator.numerator.update](./document-generator-numerator-update.md) and [documentgenerator.numerator.delete](./document-generator-numerator-delete.md) methods only work for numbering rules created via the [documentgenerator.numerator.add](./document-generator-numerator-add.md) method.

## Overview of Methods {#all-methods}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with permission to modify document generator templates

#|
|| **Method** | **Description** ||
|| [documentgenerator.numerator.add](./document-generator-numerator-add.md) | Adds a numbering rule ||
|| [documentgenerator.numerator.update](./document-generator-numerator-update.md) | Modifies a numbering rule ||
|| [documentgenerator.numerator.get](./document-generator-numerator-get.md) | Retrieves a numbering rule by identifier ||
|| [documentgenerator.numerator.list](./document-generator-numerator-list.md) | Gets a list of numbering rules ||
|| [documentgenerator.numerator.delete](./document-generator-numerator-delete.md) | Deletes a numbering rule ||
|#