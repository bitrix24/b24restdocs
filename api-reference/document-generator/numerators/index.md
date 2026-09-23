# Document Generator Numberers: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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

{% note info "" %}

The [documentgenerator.numerator.list](./document-generator-numerator-list.md) method returns the numbering rules of the document generator, including those created by the [CRM numbering rule](../../crm/document-generator/numerator/index.md) methods. Numbering rules of other tools, such as CRM invoices, are not included in the selection.

{% endnote %}

## Numberer Response Structure

The [documentgenerator.numerator.list](./document-generator-numerator-list.md) method returns the `result.numerators` array. Each item contains an identifier, a name, a number template, and generator settings. The following is an abbreviated response example:

```json
{
    "result": {
        "numerators": [
            {
                "id": "55",
                "name": "Invoice Numberer",
                "template": "INV-{NUMBER}",
                "settings": {
                    "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
                        "start": 1000,
                        "step": 1,
                        "length": 6,
                        "padString": "0",
                        "periodicBy": "year",
                        "timezone": "Europe/Berlin",
                        "isDirectNumeration": false
                    }
                }
            }
        ]
    },
    "total": 1
}
```

## Linking Numbering Rules with Other Objects

**Document Templates.** A numbering rule is linked to a template through the `numeratorId` field. To assign a numbering rule to a template, pass the numbering rule identifier to the [documentgenerator.template.add](../templates/document-generator-template-add.md) or [documentgenerator.template.update](../templates/document-generator-template-update.md) methods.

The numbering rule identifier can be obtained after creation or through the [documentgenerator.numerator.list](./document-generator-numerator-list.md) method.

## Considerations When Modifying and Deleting a Numbering Rule

The [documentgenerator.numerator.update](./document-generator-numerator-update.md) and [documentgenerator.numerator.delete](./document-generator-numerator-delete.md) methods only work for numberers created using the [documentgenerator.numerator.add](./document-generator-numerator-add.md) method. An attempt to modify or delete a numberer created in the Bitrix24 interface or through another method group returns the `DOCGEN_ACCESS_ERROR` error with the description `Access denied`.

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
