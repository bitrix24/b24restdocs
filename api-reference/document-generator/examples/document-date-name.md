# Generate a Document with Date and Name Modifiers

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Modifiers in document templates are formatting rules that control how field values, such as dates or full names, are displayed.

Input data can be pre-formatted in the application or passed through REST.

## When to Use

- You need to format a date using the document generator.
- You need to display a full name in a specified format.

## What to Include in the Request

Date and name modifiers are applied to the values of the template placeholders through the `values` and `fields` parameters of the [documentgenerator.document.add](../document-generator-document-add.md) method.

{% note tip "User Documentation" %}

- [Modifiers in document templates](https://helpdesk.bitrix24.com/open/25799327/)

{% endnote %}

### For Date

- In `values`, pass the date in Atom format, for example, `2026-03-18T00:00:00+03:00`.
- In `fields`, specify the field type: `TYPE` = `DATE`.
- If necessary, set the default output format via `FORMAT['format']`:

	- `d` — day of the month with leading zero
	- `j` — day of the month without leading zero
	- `m` — month number with leading zero
	- `n` — month number without leading zero
	- `F` — full month name
	- `y` — year in two digits
	- `Y` — year in four digits
	- `H` — hours in 24-hour format with leading zero
	- `i` — minutes with leading zero
	- `s` — seconds with leading zero

Date and time formatting symbols can be combined:

- `d.m.y` — `28.03.26`
- `j F, Y` — `28 March, 2026`
- `H:i:s` — `10:24:18`
- `Y-m-d H:i:s` — `2026-03-28 10:24:18`

### For Name

- In `values`, pass the name as an array with parts of the full name:

	```php
	[
		'NAME' => 'Igor', // first name
		'LAST_NAME' => 'Ivanov', // last name
		'SECOND_NAME' => 'Petrovich', // patronymic
		'GENDER' => 'M', // gender
	]
	```

The `GENDER` key can explicitly specify the gender: `M` or `F`. If the gender is not specified, the module will attempt to determine it based on the patronymic. If both `GENDER` and the patronymic are not provided, the gender will not be determined, and declension will not work.

- In `fields`, specify the field type: `TYPE` = `NAME`.
- In `FORMAT['format']`, you can pass the output template:

	- `#TITLE#` — salutation
	- `#NAME#` — first name
	- `#LAST_NAME#` — last name
	- `#SECOND_NAME#` — patronymic
	- `#NAME_SHORT#` — first letter of the first name with a dot
	- `#LAST_NAME_SHORT#` — first letter of the last name with a dot
	- `#SECOND_NAME_SHORT#` — first letter of the patronymic with a dot

## Example

```php
$data = [
	'templateId' => 203,
	'providerClassName' => 'Bitrix\\DocumentGenerator\\DataProvider\\Rest',
	'value' => 'ORDER_1024',
	'values' => [
		'SomeDate' => '2026-03-18T00:00:00+03:00', // value passed in atom format
		'SomeName' => [ // name passed as an array
			'NAME' => 'Vladislav',
			'LAST_NAME' => 'Gorelkin',
			'GENDER' => 'M',
		],
	],
	'fields' => [
		'SomeDate' => [
			'TYPE' => 'DATE',
			'FORMAT' => [
				'format' => 'd f Y H:i', // output format
			],
		], // field type - date
		'SomeName' => [
			'TYPE' => 'NAME',
			'FORMAT' => [ // here you can pass the default field format
				'format' => '#NAME# #LAST_NAME#' // output format
			]
		], // field type - name
	]
];
$url = $webHookUrl.'documentgenerator.document.add/';
```

## Continue Learning

- [{#T}](./document-text-data.md)
- [{#T}](./document-table-data.md)
- [{#T}](./document-table-complex.md)
- [{#T}](./document-images-seals.md)
- [{#T}](./index.md)