# Document Generation with Tabular Data

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- There should be a description and a link to the material in the general section.

{% endnote %}

{% endif %}

In our test template, there is a table where we need to insert an image, the name, and the price of the product. Modifiers for prices cannot be used in the document generator's REST, so the price will need to be passed as a formatted string.

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

Please note that a table is inserted into the template, and the table contains three fields `{TableItemName}`, `{TableItemImage}`, `{TableItemPrice}`. First, let's look at how to populate the `fields` array.

1. We pass the settings for the Table field. This field is not explicitly present in the template, but we need it to pass an array of values for the table under this key.
2. For this field, it is essential to specify the provider `fields['Table']['PROVIDER'] = 'Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider'`. This indicates that an array of values will be received under this key.
3. We need to fill in the provider's settings array. `fields['Table']['OPTIONS']['ITEM_NAME'] = 'Item'`. Here we passed the internal key that the provider will use to access the elements of the array.
4. Here we specify that each element of the `Table` field array under the key `Item` is a simple hash.
```php
fields['Table']['OPTIONS']['ITEM_PROVIDER'] = 'Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider'
```
5. We indicate that the `TableItemImage` field is an image — this is standard.

Now, regarding the `values` array.

1. Under the key `Table`, we pass a simple array (with indexed sequential keys). Each element of the array is an associative array where the key is the right part of the field, except for `Table` (the name of the array field) and `Item` (the name of the internal key).
2. As values for the table fields, we pass a chain to retrieve values from the provider. It is constructed as sequential provider codes separated by a dot. In our case, this will be `Table` — the name of the array field from which the values are obtained. Then a dot. Next, `Item` — the name of the internal key of the `Table` provider, which returns the elements of the array. Then a dot. Finally, the key of the internal associative array from the `Table` elements.
3. The `ArrayDataProvider` has an internal variable `INDEX`, which indicates the current item number (starting from 1). To insert the sequential number into the `{TableIndex}` field within the table, we specified `values['TableIndex'] = 'Table.INDEX'`.

If a regular string is specified as the value of a field, it will be inserted into the table as is in all rows.