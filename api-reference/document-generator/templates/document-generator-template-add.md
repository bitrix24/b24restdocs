# Upload Template documentgenerator.template.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.template.add` adds a new template.

The content of the file (parameter `file`) can be passed in two ways:
- In a POST request encoded in *base64* (`fields[file]`);
- Without encoding in *multipart/form-data* (just `file`);

Since the interface must be implemented independently, visibility settings (parameter `fields[users]`) are only needed for the application itself. Similarly, the sort index (parameter `fields[sort]`) and activity (parameter `fields[active]`).

All templates created through this method are linked to the rest module and the sole provider `\Bitrix\DocumentGenerator\DataProvider\Rest`.

#|
|| **Parameter** | **Description** ||
|| **fields** | Array of template fields. ||
|#

## fields Parameters

#|
|| **Parameter** | **Description** ||
|| **name**^*^ | Template name. ||
|| **file**^*^ | File content encoded in [base64](../../files/how-to-upload-files.md) (required). As an alternative, file content can be passed in `multipart/form-data`. In this case, it should not be encoded in `base64`. ||
|| **code** | Symbolic code of the template. ||
|| **numeratorId**^*^ | Identifier of the numerator. ||
|| **region**^*^ | Country. ||
|| **users** | Visibility array. By default empty. ||
|| **active** | Y/N flag for activity. By default Y. ||
|| **withStamps** | Y/N to apply stamps and signatures. By default N. ||
|| **sort** | Sort index. ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

## Example

```php
$data = [
    'fields' => [
        'name' => 'Rest Template',
        'file' => base64_encode(file_get_contents($_SERVER['DOCUMENT_ROOT'].'/upload/rest_template.docx')),
        'numeratorId' => 1,
        'region' => 'de',
        'users' => ['US'],
        'providers' => ['Bitrix\\DocumentGenerator\\DataProvider\\Rest'],
    ],
];

$url = $webHookUrl.$prefix.'.template.add/';
```

# Response on Success

> 200 OK

Returns the same data as when calling [documentgenerator.template.get()](./document-generator-template-get.md) on the new template.

```php
[result] => Array
    (
        [template] => Array
            (
                [id] => 203
                [name] => Rest Template
                [region] => de
                [code] =>
                [download] => http://mycrm.bitrix24.com/bitrix/services/main/ajax.php?action=documentgenerator.template.download&id=203&ts=1539173306
                [active] => Y
                [moduleId] => rest
                [numeratorId] => 1
                [withStamps] => N
                [providers] => Array
                    (
                        [bitrix\documentgenerator\dataprovider\rest] => bitrix\documentgenerator\dataprovider\rest
                    )

                [users] => Array
                    (
                        [US] => US
                    )

                [isDeleted] => N
                [sort] => 500
                [createTime] => 2018-10-10T14:08:26+02:00
                [updateTime] => 2018-10-10T14:08:26+02:00
                [downloadMachine] => http://mycrm.bitrix24.com/rest/1/webhookkey/documentgenerator.template.download/?token=documentgenerator%7CYWN0a // here is a long link
            )

    )
```