# Get the List of Fields for documentgenerator.template.getfields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.template.getfields` takes a template identifier as input and returns a set of template fields along with their descriptions.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Template identifier. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Example

Let's check that the template fields are recognized correctly.

```php
$data = [
    'id' => 203,
    'providerClassName' => '\\Bitrix\\DocumentGenerator\\DataProvider\\Rest',
    'value' => 1,
];
$url = $webHookUrl.$prefix.'.template.getfields/';
```

# Success Response

> 200 OK

```php
[result] => Array
    (
        [templateFields] => Array
            (
                [DocumentNumber] => Array
                    (
                        [title] => Number
                        [value] => 1
                        [group] => Array
                            (
                                [0] => Document
                            )

                        [default] => 1
                    )
                [SomeDate] => Array
                    (
                        [value] =>
                        [default] =>
                    )
                [SomeName] => Array
                    (
                        [value] =>
                        [default] =>
                    )
                [Stamp] => Array
                    (
                        [value] =>
                        [default] =>
                    )
                [Image] => Array
                    (
                        [value] =>
                        [default] =>
                    )
                [TableItemImage] => Array
                    (
                        [value] =>
                        [default] =>
                    )
                [TableItemName] => Array
                    (
                        [value] =>
                        [default] =>
                    )
                [TableItemPrice] => Array
                    (
                        [value] =>
                        [default] =>
                    )
                [TableIndex] => Array
                    (
                        [value] =>
                        [default] =>
                    )
            )
    )
```