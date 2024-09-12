# Forms in Blocks

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards

{% endnote %}

{% endif %}

Integrating Bitrix24 forms (CRM) into blocks is quite straightforward.

To do this, follow these steps:

1. In the **block** section of the [manifest](../manifest.md), add the key **subtype** with the value `form`.
2. Place a `div` with the class `bitrix24forms` inside your block. This is where the form will be displayed.
3. Add the key ext (within assets) with the value landing_form, which will include everything necessary for the forms to function.

```js
'assets' =>
    array (
        'ext' => array (
            'landing_form'
        ),
    ),
```

## Markup

The node where the form will appear must be empty. It should be marked with the class **.bitrix24forms**. You can also add two optional parameters:

- **data-b24form-use-style="Y"** - to use block styling (`Y`), or to display the original form appearance (`N`),
- **data-b24form-show-header="N"** - to hide or show the form header.

These parameters can be modified in the block settings.

### Markup Principles

Forms are embedded in `<iframe>`, meaning they are technically displayed not on the Sites, but on an external resource. With this setup, there is no way to influence the appearance of the forms from outside, from the block's side. However, when initializing forms, you can pass an array of CSS styles that will be applied to the form (inside the `<iframe>`). This is how blocks work, altering the appearance of forms and seamlessly integrating them into their design.

Create a group of styles such as "main color, main background, additional color, main border color," and so on. To do this, select a node in the block that has the desired color and mark it with a data attribute; these attributes start with the prefix **data-form-style-**. Since there may not be elements with the required color in the block, you can add hidden blocks with the necessary parameters. Generally, it looks like this:

```html
<div hidden>
<div class="g-bg-primary g-color-primary g-brd-primary"
data-form-style-main-bg="1"
data-form-style-main-border-color="1"
data-form-style-main-font-color-hover="1"
>
</div>
</div>
```

In this example, a block is added that has a primary background color, primary font color, and primary border color. The data attributes set the values for the main color/background/border color already inside the form.

{% note warning %}

If the node marked with **data-form-style-...** is accessible for design by users (for example, the block header that can be recolored), then color changes will be interactively passed to the form.

{% endnote %}

There are quite a few such attributes, and describing their purpose can be quite complex. Therefore, it is recommended to take one of the standard blocks as an example and change its settings while observing the changes.

### Example Block

```html
<section class="g-pos-rel landing-block text-center g-pt-80 g-pb-80 g-bg-primary">
    <div class="container">
        <div class="landing-block-form-styles" hidden>
            <div class="g-bg-transparent h1 g-color-white g-brd-none g-pa-0"
                data-form-style-wrapper-padding="1"
                data-form-style-bg="1"
                data-form-style-bg-content="1"
                data-form-style-bg-block="1"
                data-form-style-header-font-size="1"
                data-form-style-main-font-weight="1"
                data-form-style-border-block="1"
            >
            </div>

            <div class="g-bg-white g-color-primary g-brd-primary"
                data-form-style-main-bg="1"
                data-form-style-main-border-color="1"
                data-form-style-main-font-color-hover="1"
            >
            </div>
            <div class="g-bg-primary-dark-v2 u-theme-restaurant-shadow-v1 g-brd-around g-color-gray-dark-v2 rounded-0"
                data-form-style-input-bg="1"
                data-form-style-input-box-shadow="1"
                data-form-style-input-select-bg="1"
                data-form-style-input-border="1"
                data-form-style-input-border-radius="1"
                data-form-style-button-font-color="1"
            >
            </div>
            <div class="g-brd-around g-brd-gray-light-v2 g-brd-bottom g-bg-black-opacity-0_7"
                data-form-style-input-border-color="1"
                data-form-style-input-border-hover="1"
            >
            </div>

            <p class="g-color-white-opacity-0_7"
                data-form-style-second-font-color="1"
                data-form-style-main-font-family="1"
                data-form-style-main-font-weight="1"
                data-form-style-header-text-font-size="1">
            </p>

            <h3 class="g-font-size-11 g-color-white"
                data-form-style-label-font-weight="1"
                data-form-style-label-font-size="1"
                data-form-style-main-font-color="1"
            >
            </h3>

            <!--            for resource booking-->
            <div class="g-bg-white"
                data-form-style-bg-as-text="1"
            >
            </div>

            <div class="g-bg-primary-dark-v2"
                data-form-style-input-bg-light="1"
            >
            </div>

            <div class="g-bg-primary-dark-v3"
                data-form-style-input-bg-light2="1"
            >
            </div>

            <div class="g-bg-primary u-shadow-custom-v2"
                data-form-style-input-bg-light3="1"
                data-form-style-gradient-box-shadow="1"
            >
            </div>

            <div class="g-bg-primary-opacity-0_4"
                data-form-style-main-bg-light="1"
            >
            </div>
        </div>

        <div class="row">
            <div class="col-md-6 mx-auto">
                <div class="bitrix24forms g-brd-white-opacity-0_6 u-form-alert-v3"
                    data-b24form-use-style="Y"
                    data-b24form-show-header="N"
                ></div>
            </div>
        </div>
    </div>
</section>
```

## Example

You can view examples of blocks of this type in our repository by using the methods [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) and [landing.block.getrepository](../methods/landing-block-get-repository.md). Their codes are:
- 33.1.form_1_transparent_black_left_text
- 33.10.form_2_light_left_text
- 33.23.form_2_themecolor_no_text
- and many others

A simple example:

```js
// example of registering a simple form
BX24.callMethod(
    'landing.repo.register',
    {
        "code":"test_form",
        "fields":{"NAME":"Test form",
            "SECTIONS":"other",
            "PREVIEW":"https://restapi.bx24.net/booking/cycles_b24.jpg",
            "CONTENT":"<div class=\"bitrix24forms\"></div>"
        },
        "manifest": {
            "block":{"subtype":"form"},
            "assets":{"ext":["landing_form"]}
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}