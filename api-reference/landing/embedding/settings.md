# Settings Menu LANDING_SETTINGS

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified

{% endnote %}

{% endif %}

The embedding location **LANDING_SETTINGS** allows you to add a new item to the settings menu (Pages / Site) in the page editing mode.

## Parameters

The following parameters are available for this embedding location:

#|
|| **Parameter** | **Description** | **Available since** ||
|| **SITE_ID**
[`unknown`](../../data-types.md) | site identifier. | ||
|| **LID**
[`unknown`](../../data-types.md) | page identifier. | ||
|#

You can retrieve the parameters from PLACEMENT_OPTIONS:

```php
$placement = isset($_REQUEST['PLACEMENT_OPTIONS'])
    ? json_decode($_REQUEST['PLACEMENT_OPTIONS'], true)
    : [];
```

## Examples

```js
BX24.callMethod(
    'landing.repo.bind',
    {
        fields: {
            PLACEMENT: 'LANDING_SETTINGS',
            PLACEMENT_HANDLER: 'https://cpe/rest/settings.php',
            TITLE: 'My settings'
        }
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

{% include [Footnote on examples](../../../_includes/examples.md) %}