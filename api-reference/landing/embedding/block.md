# Editing Section of LANDING_BLOCK

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated

{% endnote %}

{% endif %}

Currently, developers have the ability to integrate into the editing sections of any block. In the case of such registration, a button to call your application will appear next to the "Edit" and "Design" buttons of the block.

The code for embedding the application depends on the block code and generally looks like this: `LANDING_BLOCK_<CODE>`. If it is clear for a system block, where you need to insert the block code instead of `<CODE>` (examples below), then for embedding into a block registered by you, you need to substitute its identifier.

For example:

1. Register a [block](../user-blocks/landing-repo-register.md). The method will return the `ID` of the block. Let's say it equals 1132 for this example.
2. When registering the embedding location, specify the code: `LANDING_BLOCK_repo_1132` (or `LANDING_BLOCK_REPO_1132`, case does not matter).

## Parameters

The following parameters are available for this embedding location:

#|
|| **Parameter** | **Description** | **Available since** ||
|| **ID**
[`unknown`](../../data-types.md) | – identifier of the block. | ||
|| **CODE**
[`unknown`](../../data-types.md) | – symbolic code of the block. | ||
|| **LID**
[`unknown`](../../data-types.md) | – identifier of the page. | ||
|#

You can obtain the parameters from PLACEMENT_OPTIONS:

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
            PLACEMENT: 'LANDING_BLOCK_04.1.one_col_fix_with_title',
            PLACEMENT_HANDLER: 'https://cpe/rt/placement.php',
            TITLE: 'My block'
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

If you want to embed a universal application for every block, you should specify the code with *:

```js
BX24.callMethod(
    'landing.repo.bind',
    {
        fields: {
            PLACEMENT: 'LANDING_BLOCK_*',
            PLACEMENT_HANDLER: 'https://cpe/rt/placement.php',
            TITLE: 'My block'
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
)
```

## Updating a Block from the Application

In the opened application, there is a command to update a specific block. It is assumed that after working with the block from which the application was called, you may need to refresh it. This is done through the refreshBlock command.

### Example

```js
BX24.placement.call(
    'refreshBlock',
    {
        id: 123 // block with identifier 123
    },
    function()
    {
        console.log('Block successfully updated');
        // close the slider
    }
);
```

{% include [Examples Note](../../../_includes/examples.md) %}