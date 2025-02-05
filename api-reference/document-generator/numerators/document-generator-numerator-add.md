# Add Numerator documentgenerator.numerator.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.numerator.add` adds a new numerator.

#|
|| **Parameter** | **Description** ||
|| **name** | Name. ||
|| **template** | Template. ||
|| **settings** | Generator settings. ||
|#

## Example

```php
\Bitrix\Main\Loader::includeModule('rest');
$client = new \Bitrix\Main\Web\HttpClient();
$webHookUrl = 'http://mycrm.bitrix24.com/rest/1/webhookkey/';
$prefix = 'documentgenerator';

$data = [
    'fields' => [
        'name' => 'Rest Numerator',
        'template' => '{NUMBER}',
        'settings' => [
            'Bitrix_Main_Numerator_Generator_SequentNumberGenerator' => [
                'start' => '0',
                'step' => '1',
            ],
        ],
    ],
];

$url = $webHookUrl.$prefix.'.numerator.add/';
$answer = $client->post($url, $data);
try
{
    $result = \Bitrix\Main\Web\Json::decode($answer);
}
catch(Exception $e)
{
    var_dump($answer);
}

print_r($result);
```

# Response in case of success

Returns a result identical to [documentgenerator.numerator.get()](./document-generator-numerator-get.md).

> 200 OK

```php
[result] => Array
    (
        [numerator] => Array
            (
                [name] => new rest numerator
                [template] => {NUMBER}
                [id] => 68
                [settings] => Array
                    (
                        [Bitrix_Main_Numerator_Generator_SequentNumberGenerator] => Array
                            (
                                [start] => 0
                                [step] => 1
                                [periodicBy] =>
                                [timezone] =>
                                [isDirectNumeration] =>
                            )

                    )

            )
    )
```

From the response, we obtain the `id` of the numerator and can use it further.