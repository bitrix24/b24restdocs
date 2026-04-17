# Generate a Document with Text

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

If you need to insert only simple text data into the document, you should pass an array of `values` with text values to the method [documentgenerator.document.add](../document-generator-document-add.md):

```php
$data = [
    'templateId' => 203,
    'providerClassName' => 'Bitrix\\DocumentGenerator\\DataProvider\\Rest',
    'value' => 1,
    'values' => [
        'SomeDate' => '02/14/2018',
        'SomeName' => 'Vladislav Gorelkin',
    ],
];
$url = $webHookUrl.$prefix.'.document.add/';
```