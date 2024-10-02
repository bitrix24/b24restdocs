# Unregister Widget for Vibe landing.repowidget.unregister

{% note warning "We are still working on the tool" %}

The functionality will be released soon.

{% endnote %}

> Scope: [`landing`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.repowidget.unregister` removes the widget for Vibe - the main page. On success, it returns `true`; otherwise, it returns `false` or an error with a description.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`string`](../data-types.md) | Unique code of the widget to be removed ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.repowidget.unregister', {
            code: 'my_widget'
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.repowidget.unregister',
        [
            'code' => 'my_widget'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1713949410.036288,
        "finish": 1713949411.632775,
        "duration": 1.596487045288086,
        "processing": 0.6458539962768555,
        "date_start": "2024-04-24T11:03:30+02:00",
        "date_finish": "2024-04-24T11:03:31+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Result of the widget removal ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-repowidget-register.md)