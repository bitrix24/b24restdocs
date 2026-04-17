# Generate a Document with Tabular Data

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

In our test template, there is a table where you need to insert an image, the name, and the price of the product. Modifiers for prices cannot be used in the document generator's REST, so the price will need to be passed as a formatted string.

Below is a working example of the code, followed by details.

```php
$data = [
	'templateId' => 203,
	'providerClassName' => '\\Bitrix\\DocumentGenerator\\DataProvider\\Rest',
	'value' => 1,
	'values' => [
		'Table' => [
			[
				'Name' => 'Item name 1',
				'Price' => '$111.23',
				'Image' => 'http://myrestapp.com/upload/stamp.png'
			],
			[
				'Name' => 'Item name 2',
				'Price' => '$222.34',
				'Image' => 'http://myrestapp.com/upload/stamp.png'
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
$url = $webHookUrl.$prefix.'.document.add/';
```

Please note that a table is inserted into the template, which contains three fields: `{TableItemName}`, `{TableItemImage}`, and `{TableItemPrice}`. First, let's look at how the `fields` array is populated.

1. We pass the settings for the Table field. This field is not explicitly present in the template, but we need it to pass an array of values for the table under this key.
2. For this field, it is essential to specify the provider: `fields['Table']['PROVIDER'] = 'Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider'`. This indicates that an array of values will be received under this key.
3. We need to fill in the provider's options array: `fields['Table']['OPTIONS']['ITEM_NAME'] = 'Item'`. Here, we have provided the internal key that the provider will use to access the elements of the array.
4. This specifies that each element of the `Table` field array under the key `Item` is a simple hash.
```php
fields['Table']['OPTIONS']['ITEM_PROVIDER'] = 'Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider'
```
5. We indicate that the `TableItemImage` field is an image — this is standard.

Now, regarding the `values` array.

1. Under the `Table` key, we pass a simple array (with sequential index keys). Each element of the array is an associative array where the key is the right part of the field, excluding `Table` (the name of the array field) and `Item` (the name of the internal key).
2. As values for the table fields, we pass a chain to retrieve values from the provider. This is constructed as sequential provider codes separated by a dot. In our case, it will be `Table` — the name of the array field from which the values are obtained. Then a dot. Next, `Item` — the name of the internal key of the `Table` provider, which returns the elements of the array. Then a dot. Finally, the key of the internal associative array from the `Table` elements.
3. The `ArrayDataProvider` has an internal variable `INDEX`, which indicates the current item number (starting from 1). To insert the sequential number into the `{TableIndex}` field within the table, we specify `values['TableIndex'] = 'Table.INDEX'`.

If a regular string is specified as the value for a field, it will be inserted into the table as is in all rows.