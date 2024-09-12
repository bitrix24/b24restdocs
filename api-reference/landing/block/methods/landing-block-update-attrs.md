# Change Attributes of the Node Block landing.block.updateattrs

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter mandatory status is not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.updateattrs` modifies the attributes of the node block. It returns _true_ or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **data**
[`unknown`](../../../data-types.md) | An array of selectors and new values for data attributes.
For example, `data: {'.bitrix24forms': {'data-b24form': 'tratrata'}}`.
The manifest must contain [attributes](../manifest.md#key-attrs) that you want to modify in this way. | ||
|#

If the attribute pertains to a detail form (meaning it can have different content from one detail form to another), the selector must be passed with a separator @:

```http
data: {
    '.container-fluid@1': {//the change will affect the attribute of the second detail form (zero-based index)
        'data-test-checkbox': [1, 2, 3]
    }
}
```

## Types of Modifiable Content

Each type of attribute has its own format for saving. The examples provide default values for each [type](../attributes.md#attribute-types). The new value is transmitted in a similar format. For example, saving to an attribute of type **image**:

```http
data: {
    '.container-fluid': {
        'data-test-image': {src: 'https://i.img.com/images/i/291626458734-0-1/s-l1000.jpg', alt: 666}
    }
}
```

Specific notes for the **checkbox** and **multiselect** types: to save a new value, you need to send the values of the selected items:

```http
data: {
    '.container-fluid': {
        'data-test-checkbox': [1, 2, 3]
    }
}
```

Editing parameters of dynamic blocks is done through the method [landing.block.updatenodes](./landing-block-update-nodes.md).

## Examples

```js
BX24.callMethod(
    'landing.block.updateattrs',
    {
        lid: 313,
        block: 6134,
        data: {
            '.bitrix24forms': {
                'data-b24form': 'tratrata'
            }
        }
    },
    function(result)
    {
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            console.info(result.data());
        }
    }
);
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}