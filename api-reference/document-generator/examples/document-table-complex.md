# Generate a Document with Complex Tables

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In the REST document generator, you can fill template placeholders with nested lists, where one of the first-level list values contains another list. The method used to create the document is [documentgenerator.document.add](../document-generator-document-add.md).

## When to Use

- When you need to insert multiple tables of the same structure but with a different number of rows
- When you need to build a table within a repeating block

In such scenarios, the outer list defines the structure, while the inner lists are passed as separate fields and linked through chains like `Events.Event.Title` and `Event1Speakers.Speaker.Name`.

## What to Pass in the Request

- In `values`, pass the outer list, for example, `Events`
- In the elements of the outer list, pass placeholders for the related inner lists, for example, `{Event1SpeakersSpeakerName}`
- For the fields of the outer list, pass access chains like `Events.Event.Title`
- For each inner list, pass a separate array in `values`, for example, `Event1Speakers`
- For the fields of the inner lists, pass access chains like `Event1Speakers.Speaker.Name`
- In `fields`, describe the outer and inner lists through `ArrayDataProvider`
- In the `OPTIONS` of each list, specify `ITEM_NAME` and `ITEM_PROVIDER = Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider`

## Points to Note

- In `fields`, the outer list should come before the related inner lists so that the providers are resolved in the correct order
- The codes of the inner lists must match the placeholders passed in the outer list

## Example of a Table Inside Repeating Blocks

```php
$data = [
	'templateId' => 203,
	'providerClassName' => 'Bitrix\\DocumentGenerator\\DataProvider\\Rest',
	'value' => 'EVENTS_2026',
	'values' => [
		'Title' => 'Welcome to my template',
		'Description' => '<b>Description is here</b>',
		'Picture' => null, // you can pass a link to an image
		'Events' => [
			[
				'Title' => 'Automation',
				'Description' => 'Some description of the automation event',
				'SpeakerName' => '{Event1SpeakersSpeakerName}',
				'SpeakerCompany' => '{Event1SpeakersSpeakerCompany}',
				'SpeakerPosition' => '{Event1SpeakersSpeakerPosition}',
			],
			[
				'Title' => 'Documents',
				'Description' => 'This event is about document processing',
				'SpeakerName' => '{Event2SpeakersSpeakerName}',
				'SpeakerCompany' => '{Event2SpeakersSpeakerCompany}',
				'SpeakerPosition' => '{Event2SpeakersSpeakerPosition}',
			],
		],
		'EventsEventTitle' => 'Events.Event.Title',
		'EventsEventDescription' => 'Events.Event.Description',
		'EventsEventSpeakerName' => 'Events.Event.SpeakerName',
		'EventsEventSpeakerCompany' => 'Events.Event.SpeakerCompany',
		'EventsEventSpeakerPosition' => 'Events.Event.SpeakerPosition',
		'Event1SpeakersSpeakerName' => 'Event1Speakers.Speaker.Name',
		'Event1SpeakersSpeakerCompany' => 'Event1Speakers.Speaker.Company',
		'Event1SpeakersSpeakerPosition' => 'Event1Speakers.Speaker.Position',
		'Event1Speakers' => [
			[
				'Name' => 'John Smith',
				'Company' => 'Cool Ltd.',
				'Position' => 'Core developer',
			],
			[
				'Name' => 'Michael Johnson',
				'Company' => 'Cool Ltd.',
				'Position' => 'Product Manager',
			],
		],
		'Event2Speakers' => [
			[
				'Name' => 'David Brown',
				'Company' => 'Devils corp.',
				'Position' => 'Chief',
			],
		],
		'Event2SpeakersSpeakerName' => 'Event2Speakers.Speaker.Name',
		'Event2SpeakersSpeakerCompany' => 'Event2Speakers.Speaker.Company',
		'Event2SpeakersSpeakerPosition' => 'Event2Speakers.Speaker.Position',
	],
	'fields' => [
		'Events' => [
			'PROVIDER' => 'Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider',
			'OPTIONS' => [
				'ITEM_NAME' => 'Event',
				'ITEM_PROVIDER' => 'Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider',
			],
		],
		'Event1Speakers' => [
			'PROVIDER' => 'Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider',
			'OPTIONS' => [
				'ITEM_NAME' => 'Speaker',
				'ITEM_PROVIDER' => 'Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider',
			],
		],
		'Event2Speakers' => [
			'PROVIDER' => 'Bitrix\\DocumentGenerator\\DataProvider\\ArrayDataProvider',
			'OPTIONS' => [
				'ITEM_NAME' => 'Speaker',
				'ITEM_PROVIDER' => 'Bitrix\\DocumentGenerator\\DataProvider\\HashDataProvider',
			],
		],
		'Event1SpeakersSpeakerName' => ['TITLE' => 'Event1SpeakersSpeakerName'],
		'Event1SpeakersSpeakerCompany' => ['TITLE' => 'Event1SpeakersSpeakerCompany'],
		'Event1SpeakersSpeakerPosition' => ['TITLE' => 'Event1SpeakersSpeakerPosition'],
		'Event2SpeakersSpeakerName' => ['TITLE' => 'Event2SpeakersSpeakerName'],
		'Event2SpeakersSpeakerCompany' => ['TITLE' => 'Event2SpeakersSpeakerCompany'],
		'Event2SpeakersSpeakerPosition' => ['TITLE' => 'Event2SpeakersSpeakerPosition'],
	],
];
$url = $webHookUrl.'documentgenerator.document.add/';
```

## Continue Your Learning

- [{#T}](./document-text-data.md)
- [{#T}](./document-date-name.md)
- [{#T}](./document-table-data.md)
- [{#T}](./document-images-seals.md)
- [{#T}](./index.md)