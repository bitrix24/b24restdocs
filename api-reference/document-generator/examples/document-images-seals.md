# Document Generation with Images and Stamps

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- There should be a description and a link to the material in the general section.

{% endnote %}

{% endif %}

Let's consider a scenario where we need to insert images, a stamp, use modifiers for the date and name, and fill a table with several values.

To pass complex data to the document, not just in string format, it is necessary to correctly form the `fields` parameter. First, let's fill in the image and stamp.

```php
$data = [
    'templateId' => 203,
    'providerClassName' => 'Bitrix\\DocumentGenerator\\DataProvider\\Rest',
    'value' => 1,
    'values' => [
        'SomeDate' => '02/14/2018',
        'SomeName' => 'Vladislav Gorelkin',
        'Stamp' => 'http://myrestapp.com/upload/stamp.png', // external path to the stamp file
        'Image' => 'http://myrestapp.com/upload/image.jpg', // external path to the image file
    ],
    'fields' => [
        'Stamp' => ['TYPE' => 'STAMP'], // field type - stamp
        'Image' => ['TYPE' => 'IMAGE'], // field type - image
    ]
];
$url = $webHookUrl.$prefix.'.document.add/';
```

In the `fields` array, you can specify the field type using the `TYPE` key:
- for "Image" fields, the type is `STAMP`
- for "Stamp or Signature" fields, the type is `IMAGE`

In the `values` array, you need to specify the absolute path to the file as values. The file will be downloaded from this address and inserted into the document.