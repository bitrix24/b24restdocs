# Typical use-cases and scenarios of the document generator: case overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with document creation permissions

A template in the document generator is a `.docx` file with placeholders for fields that are replaced with data from the request when creating the document. Placeholders are formatted in curly braces, such as `{DocumentNumber}` or `{MyField}`. For more details, refer to the article [{#T}](../templates/index.md).

The cases help prepare data for document creation based on the template using the [documentgenerator.document.add](../document-generator-document-add.md) method. After creating the document, you can obtain links to the files and continue working with the results using methods from the [{#T}](../index.md) section.

> Quick navigation: [all cases](#all-cases)

## Getting Started

1. Prepare a `.docx` template with field placeholders.
2. Retrieve the field map of the template using the [documentgenerator.template.getfields](../templates/document-generator-template-get-fields.md) method.
3. Determine which data needs to be passed in `values` and `fields` for the [documentgenerator.document.add](../document-generator-document-add.md) method.
   - `values` — what to insert into the document.
   - `fields` — how to interpret this data: field type, format, image, print, table, or repeating block.
4. Open the example that is closest to your scenario and adapt it to your template.

## What Is Returned in the Response

All cases end with a call to [documentgenerator.document.add](../document-generator-document-add.md), so their response is the same: a `document` object with the document identifier, its number, and links to the files.

- `id` — document identifier for further calls
- `downloadUrl` and `downloadUrlMachine` — DOCX download links for the user and for the application
- `isTransformationError` — flag indicating a conversion error

The `pdfUrl` and `imageUrl` fields are returned in the response only after the PDF and the document image have been generated. The conversion is performed asynchronously, so these fields are usually absent immediately after creation — check them with the [documentgenerator.document.get](../document-generator-document-get.md) method.

If the request fails, the method returns an error.

```json
{
    "error": "0",
    "error_description": "Cannot create document on deleted template"
}
```

A complete description of the response fields and the list of errors is available on the page of the [documentgenerator.document.add](../document-generator-document-add.md) method. Each case covers only the errors typical of its scenario.

## Case Overview {#all-cases}

#|
|| **Case** | **Description** ||
|| [Generate a document with text](./document-text-data.md) | Inserts text values without additional field type settings ||
|| [Generate a document with date and name modifiers](./document-date-name.md) | Formats date and name through `DATE` and `NAME` field types ||
|| [Generate a document with tabular data](./document-table-data.md) | Fills rows of a single table with a repeating structure ||
|| [Generate a document with complex tables](./document-table-complex.md) | Works with nested tables and multiple repeating blocks ||
|| [Generate a document with images and seals](./document-images-seals.md) | Inserts images, seals, and signatures via external links ||
|#