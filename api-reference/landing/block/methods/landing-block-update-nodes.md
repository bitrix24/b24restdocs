# Change the content of the block landing.block.updatenodes

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

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

The method `landing.block.updatenodes` changes the content of the block. It returns _true_ or an error. This method is also used for [updating the parameters of dynamic blocks](#edit_params), such as the product list, detailed product, and some others.

## Parameters

#|
|| **Method** | **Description** | **Version** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier | ||
|| **block**
[`unknown`](../../../data-types.md) | Block identifier | ||
|| **data**
[`unknown`](../../../data-types.md) | Array of selectors and new values.
For example: `data: {'.landing-block-node-text@1': 'new text!'}`. The principle is the same - selector and its new value. If you are sure that the selector in the block is unique, you can omit the counter **@1**.
Also, data depends on the types of nodes being modified. For more details, see the example below; for descriptions of types, refer to [a separate page](../node-types.md). | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.block.updatenodes',
        {
            lid: 311,
            block: 6058,
            data: {
                '.landing-block-node-text': 'Text with html',
                '.landing-block-node-img': {src: '/some/path/picture.png', alt: 'My picture'},
                '.landing-block-node-link': {text: 'My link', href: 'https://bitrix24.com', target: '_blank'},
                '.landing-block-node-icon': ['fa-telegram', 'fa-skype'],
                '.landing-block-node-embed': {src: '//www.youtube.com/embed/q4d8g9Dn3ww?autoplay=1&controls=0&loop=1&mute=1&rel=0', source: 'https://www.youtube.com/watch?v=q4d8g9Dn3ww'},
            },
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

## Editing parameters of dynamic blocks

There are several dynamic blocks whose parameters can be changed via REST. For example, the number of products on a page. This can be done as follows.

1. Use the method [landing.block.getmanifest](./landing-block-get-manifest.md) to find out what parameters the block has. The method will return an array of the manifest, where we are interested in the key `attrs` and the parameters of the dynamic block component you are interested in. In this case, we are interested in `bitrix:catalog.section`.

    ```js
    attrs:
    bitrix:catalog.section: Array(24)
    0: {name: "Section ID", style: false, original_type: "component", component_type: "STRING", attribute: "SECTION_ID", …}
    1: {name: "Unavailable products", style: false, original_type: "component", component_type: "LIST", attribute: "HIDE_NOT_AVAILABLE", …}
    2: {name: "Unavailable trade offers", style: false, original_type: "component", component_type: "LIST", attribute: "HIDE_NOT_AVAILABLE_OFFERS", …}
    3: {name: "Field by which we sort elements", style: false, original_type: "component", component_type: "LIST", attribute: "ELEMENT_SORT_FIELD", …}
    4: {name: "Order of sorting elements", style: false, original_type: "component", component_type: "LIST", attribute: "ELEMENT_SORT_ORDER", …}
    5: {name: "Currency to which prices will be converted", style: false, original_type: "component", component_type: "LIST", attribute: "CURRENCY_ID", …}
    6: {name: "Price type", style: false, original_type: "component", component_type: "LIST", attribute: "PRICE_CODE", …}
    ...
    ```

2. Use the method `landing.block.updatenodes` to change the necessary parameters. Historically, dynamic parameters (attributes) are changed specifically through this method.

    {% list tabs %}

    - JS

        ```js
        BX24.callMethod(
            'landing.block.updatenodes',
            {
                lid: 5597,
                block: 44131,
                data: {
                    'bitrix:catalog.section': {
                        attrs: {
                            'MESS_BTN_BUY': 'Add to my cart'
                        }
                    }
                },
                function(result)
                {
                    if (result.error())
                    {
                        console.error(result.error());
                    }
                    else
                    {
                        console.info(result.data());
                    }
                }
            }
        );
        ```

    {% endlist %}