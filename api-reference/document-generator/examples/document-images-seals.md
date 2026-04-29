# Generate a Document with Images and Stamps

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Images, stamps, and signatures for template placeholders are passed to the method [documentgenerator.document.add](../document-generator-document-add.md) as file links in `values`. The files are downloaded from the specified URL and inserted into the document during generation.

## When to Use

- When you need to insert an image from an external link
- When you need to add a stamp or signature in a template field

## What to Include in the Request

- In `values`, provide absolute URLs for the files. The file URL must be accessible by Bitrix24 without additional authorization.
- In `fields`, specify the type for the field code:
  - `IMAGE` — image field
  - `STAMP` — stamp or signature field
- The field codes in `values` and `fields` must match the placeholder codes in the template.

## Example

```php
$data = [
    'templateId' => 203,
    'providerClassName' => 'Bitrix\\DocumentGenerator\\DataProvider\\Rest',
    'value' => 'ORDER_1024',
    'values' => [
        'Stamp' => 'https://myrestapp.example/upload/stamp.png', // external path to the stamp file
        'Image' => 'https://myrestapp.example/upload/image.jpg', // external path to the image file
    ],
    'fields' => [
        'Stamp' => ['TYPE' => 'STAMP'], // field type - stamp
        'Image' => ['TYPE' => 'IMAGE'], // field type - image
    ]
];
$url = $webHookUrl.'documentgenerator.document.add/';
```

## Continue Learning

- [{#T}](./document-text-data.md)
- [{#T}](./document-date-name.md)
- [{#T}](./document-table-data.md)
- [{#T}](./document-table-complex.md)
- [{#T}](./index.md)