# Change Styles of Block landing.block.updateStyles

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.updateStyles` changes the styles of the block. It returns _true_ or an error.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **data**
[`unknown`](../../../data-types.md) | This parameter passes an array of key-value pairs, where the key is the selector, and each value specifies two arrays:
- **classList** - which classes to add to the modified selector.
- **affect** - styles that need to be reset for all child nodes are passed. For example, if a class that colors the element (color) is passed, then in affect, an array [color] should be passed to reset all colors for the children. Otherwise, there will be a situation where the parent's color is red, but the text inside remains unchanged.

The selector can be passed without specifying a position (for example, .landing-block-node-text), in which case all cards matching this selector will be modified. It can also be passed with a position specified (for example, .landing-block-node-text@2), in which case only the card at the specified position (zero-based) will be modified.

The selector can be passed as `#wrapper`, in which case the influence will occur on the styles of the block (its shell). | ||
|#

## Example

In this example, text-right is a **class that aligns text to the right**. Therefore, in affect, it is specified that all underlying text-align styles should be removed.

{% note warning %}

Classes such as landing-block-node-text are system classes in the manifest. If you do not pass them, the class will be lost, and the node will not be able to change through the visual interface. You must clearly understand what you are doing.

{% endnote %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.block.updateStyles',
        {
            lid: 311,
            block: 6058,
            data: {
                '.landing-block-node-text': {
                    classList: ['landing-block-node-text', 'g-color-gray-light-v2', 'text-right'],
                    affect: ['text-align']
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

{% endlist %}



{% include [Footnote about examples](../../../../_includes/examples.md) %}