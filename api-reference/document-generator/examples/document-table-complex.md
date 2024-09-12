# Document Generation with Complex Tables

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- There should be a description and a link to the material in the general section.

{% endnote %}

{% endif %}

Starting from version 18.7.200 of the `documentgenerator` module, it is now possible to create documents with nested lists, meaning that one of the values in the first-level list can be another list (**Note:** this only works through REST and PHP API, it cannot be done through the interface).

This can be useful when you need to insert multiple tables of the same structure but with a different number of rows.

The main idea is that the **first-level list** should return **field names** for the inner lists as values. All descriptions and field values must be provided at once. In the field array, the outer list should come first (since fields are processed in the same order they are provided in the description).

This method can be used in the **Table inside repeating blocks** or **Repeating blocks inside repeating blocks** schemes, but you cannot insert a table inside a table.

## Example for the Table inside Repeating Blocks Scheme

Upload the template using the [documentgenerator.template.add](../templates/index.md) method.

After that, you can create a document that will contain two tables. Example input data:

```php
// Description of values
'values' => [
	'Title' => 'Welcome to my template',
	'Description' => '<b>Description is here</b>',
	'Picture' => null, // you can put here a link to an image
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
			'Company' => 'Devils Corp.',
			'Position' => 'Chief',
		],
	],
	'Event2SpeakersSpeakerName' => 'Event2Speakers.Speaker.Name',
	'Event2SpeakersSpeakerCompany' => 'Event2Speakers.Speaker.Company',
	'Event2SpeakersSpeakerPosition' => 'Event2Speakers.Speaker.Position',
]
```

```php
// Description of fields
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
```

## Example for the Repeating Blocks inside Repeating Blocks Scheme

Upload the template using the [documentgenerator.template.add](../templates/index.md) method.

After that, you can create a document. Example input data:

```php
<?php
require_once($_SERVER['DOCUMENT_ROOT'].'/rest_test/rest.php');
$templateId = 58;
$entityTypeId = 2;
$entityid = 28;
$action = new RestAction('document', 'add');
$action->setPrefix('crm.documentgenerator');
$data = [
	'templateId' => $templateId,
	'entityTypeId' => $entityTypeId,
	'entityId' => $entityid, 
	'values' => [
		'Title' => 'Welcome to my template',
		'Description' => '<b>Description is here</b>',
		'Picture' => null, // you can put here a link to an image
		'Events' => [
			[
				'Title' => 'Automation',
				'Description' => 'Some description of the automation event',
				'SpeakerName' => '{Event1SpeakersSpeakerName}',
				'BlockStart' => '{Event1Speakers.BLOCK_START}',
				'BlockEnd' => '{Event1Speakers.BLOCK_END}',
				'SpeakerCompany' => '{Event1SpeakersSpeakerCompany}',
				'SpeakerPosition' => '{Event1SpeakersSpeakerPosition}',
			],
			[
				'Title' => 'Documents',
				'Description' => 'This event is about document processing',
				'BlockStart' => '{Event2Speakers.BLOCK_START}',
				'BlockEnd' => '{Event2Speakers.BLOCK_END}',
				'SpeakerName' => '{Event2SpeakersSpeakerName}',
				'SpeakerCompany' => '{Event2SpeakersSpeakerCompany}',
				'SpeakerPosition' => '{Event2SpeakersSpeakerPosition}',
			],
		],
		// 'Events' => [
		//	[
		//		'Title' => 'Automation',
		//		'Description' => 'Some description of the automation event',
		//		'SpeakerName' => '__Event1SpeakersSpeakerName__',
		//		'BlockStart' => '__Event1Speakers.BLOCK_START__',
		//		'BlockEnd' => '__Event1Speakers.BLOCK_END__',
		//		'SpeakerCompany' => '__Event1SpeakersSpeakerCompany__',
		//		'SpeakerPosition' => '__Event1SpeakersSpeakerPosition__',
		//	],
		//	[
		//		'Title' => 'Documents',
		//		'Description' => 'This event is about document processing',
		//		'SpeakerName' => '__Event2SpeakersSpeakerName__',
		//		'BlockStart' => '__Event2Speakers.BLOCK_START__',
		//		'BlockEnd' => '__Event2Speakers.BLOCK_END__',
		//		'SpeakerCompany' => '__Event2SpeakersSpeakerCompany__',
		//		'SpeakerPosition' => '__Event2SpeakersSpeakerPosition__',
		//	],
		//],
		'EventsEventTitle' => 'Events.Event.Title',
		'EventsEventDescription' => 'Events.Event.Description',
		'EventsEventSpeakerName' => 'Events.Event.SpeakerName',
		'EventsEventBlockStart' => 'Events.Event.BlockStart',
		'EventsEventBlockEnd' => 'Events.Event.BlockEnd',
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
				'Company' => 'Devils Corp.',
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
		'Event2SpeakersSpeakerPosition' => ['TITLE' => 'Event2SpeakersSpeakerPosition']
	]
];
$result = $action->run($data, 'document');
echo '<pre>';
print_r($result);
echo '</pre>';
```