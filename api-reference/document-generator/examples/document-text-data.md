# Generate a Document with Text

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Text values for the template placeholders are passed to the method [documentgenerator.document.add](../document-generator-document-add.md) via the `values` parameter without additional field type settings.

## When to Use

- The template contains only text placeholders without type modifiers.
- There is no need to specify `TYPE`, `FORMAT`, and providers in `fields`.

## What to Pass in the Request

- `values` — an object of the form `"FieldCode": "TextValue"`
- `fields` can be omitted if all fields are inserted as plain text without formatting.

Keys in `values` must match the field codes from the template; for example, for the placeholder `{SomeName}`, you need to pass `'SomeName'`.

You can obtain the field codes of the template using the method [documentgenerator.template.getfields](../templates/document-generator-template-get-fields.md).

For REST calls, the provider `Bitrix\\DocumentGenerator\\DataProvider\\Rest` is used.

## Example

```php
$data = [
    'templateId' => 203,
    'providerClassName' => 'Bitrix\\DocumentGenerator\\DataProvider\\Rest',
    'value' => 'ORDER_1024',
    'values' => [
        'DocumentNumber' => 'DG-2026-001',
        'CurrentDate' => '03/18/2026',
        'ClientName' => 'Acme Corp',
        'Comment' => 'Payment within 5 business days after signing',
    ],
];
$url = $webHookUrl.'documentgenerator.document.add/';
```

## Continue Learning

- [{#T}](./document-date-name.md)
- [{#T}](./document-table-data.md)
- [{#T}](./document-table-complex.md)
- [{#T}](./document-images-seals.md)
- [{#T}](./index.md)