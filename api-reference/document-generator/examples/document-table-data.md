# Generate a Document with Tabular Data

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Tabular data for template placeholders is passed to the method [documentgenerator.document.add](../document-generator-document-add.md) as an array of strings in `values`. In `fields`, you need to specify the table provider so that the generator processes the array as repeating rows.

## When to Use

- When you need to fill a single table with rows of the same structure
- Each row of the table contains a simple set of values, such as name, price, and image
- When you need to display the row number within the table

## What to Pass in the Request

- In `fields['Table']['PROVIDER']`, specify `Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider` so that the generator processes `values['Table']` as a list of table rows
- In `fields['Table']['OPTIONS']`, specify:
  - `ITEM_NAME` — the internal name of the array element
  - `ITEM_PROVIDER` — `Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider`
- In `values['Table']`, pass the list of table rows
- For table placeholders, such as `TableItemName` and `TableItemPrice`, provide the data access chain: `Table.Item.Name`, `Table.Item.Price`
- For images in the table, specify `TYPE = IMAGE` in `fields`
- For the row number, you can use `Table.INDEX`

## Example

```php
$data = [
	'templateId' => 203,
	'providerClassName' => 'Bitrix\\DocumentGenerator\\DataProvider\\Rest',
	'value' => 'ORDER_1024',
	'values' => [
		'Table' => [
			[
				'Name' => 'Item name 1',
				'Price' => '$111.23',
				'Image' => 'https://myrestapp.example/upload/product-1.png'
			],
			[
				'Name' => 'Item name 2',
				'Price' => '$222.34',
				'Image' => 'https://myrestapp.example/upload/product-2.png'
			],
		],
		'TableItemName' => 'Table.Item.Name',
		'TableItemImage' => 'Table.Item.Image',
		'TableItemPrice' => 'Table.Item.Price',
		'TableIndex' => 'Table.INDEX',
	],
	'fields' => [
		'Table' => [
			'PROVIDER' => 'Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider',
			'OPTIONS' => [
				'ITEM_NAME' => 'Item',
				'ITEM_PROVIDER' => 'Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider',
			],
		],
		'TableItemImage' => ['TYPE' => 'IMAGE'],
	],
];
$url = $webHookUrl.'documentgenerator.document.add/';
```

## How It Works

In the example, it is assumed that the template contains a table with fields `{TableItemName}`, `{TableItemImage}`, `{TableItemPrice}`.

1. The `Table` field is used as a container for the array of rows. This placeholder may not exist in the template, but it is necessary to pass the array of values for the table.
2. By `ITEM_NAME = Item`, the provider reads each row element as an `Item` object.
3. By `ITEM_PROVIDER = HashDataProvider`, the elements are read as a simple hash.
4. Fields like `TableItem...` reference values through the chain `Table.Item.<Key>`, where `<Key>` is the key of the internal associative array, such as `Name`, `Price`, or `Image`.

`Table.INDEX` returns the current row number, starting from `1`.

If a regular string is specified as the value of a field, it will be inserted into the table as is in all rows.

## Continue Learning

- [{#T}](./document-text-data.md)
- [{#T}](./document-date-name.md)
- [{#T}](./document-table-complex.md)
- [{#T}](./document-images-seals.md)
- [{#T}](./index.md)