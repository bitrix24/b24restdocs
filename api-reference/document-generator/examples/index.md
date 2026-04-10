# Generating a Simple Document 

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- there should be a description and a link to the material in the general section

{% endnote %}

{% endif %}

If the document needs to include only simple text data, you should pass an array `values` with text values to the method [documentgenerator.document.add](../document-generator-document-add.md):

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