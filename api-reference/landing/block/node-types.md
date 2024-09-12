# Node Types

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard

{% endnote %}

{% endif %}

As you can learn from the [manifest file](./manifest.md), there are several types of nodes – containers that hold various types of content. Let's take a look at what they are at the moment. The examples show how to register in the manifest under the **nodes** key, and how it appears in the layout.

## text

Regular text content with minimal inline HTML.

```js
'.landing-block-node-card-title' => array(
    'name' => 'Description',
    'type' => 'text',
),
```

```js
<h2 class="landing-block-node-card-title">Company24 video</h2>
```

## img

Image. It can be used as a separate tag (`<img>`), or as a background (usually for the `<div>` tag). For this type, it is also necessary to specify the recommended size. Why is this done? Images can serve different purposes. An avatar can be very small, while a background can be quite large. Moreover, if content editors upload huge images for the avatar, they will quickly exhaust the available space on the account, and the page will lag in the browser. Therefore, block authors need to take care of declaring these nodes themselves. If the size is not specified, the system will resize such images to a uniform small size.

```php
'.landing-block-node-card-bgimg' => array(
    'name' => 'Background image',
    'type' => 'img',
    // the image will be resized to this size
    'dimensions' => array('width' => 1920, 'height' => 1080),
    // the system will reduce the image only if it exceeds the size
    'dimensions' => array('maxWidth' => 1920, 'maxHeight' => 1080),
    // the system will enlarge the image until the width
    // or height matches the specified
    'dimensions' => array('minWidth' => 1920, 'minHeight' => 1080),
),
```

```html
<div class="landing-block-node-card-bgimg"></div>
```

{% note warning %}

This type must have an attribute with the image. For the `img` tag, this is the **src** attribute, and it obviously should not be empty. For the **style** attribute (in the `div` tag), it is **background-image**.

{% endnote %}

## link

Link, which contains the link text, the link value, the link type (link, phone, ...), as well as the type of opening (in the current window, new, ...).

```js
'.landing-block-node-card-button' => array(
    'name' => 'Button',
    'type' => 'link',
),
```

```js
<a href="/"
    class="landing-block-node-card-button text-uppercase btn u-btn-outline-white btn-md rounded-0">
    Read more
</a>
```

If the link is not text-based (for example, contains an image or something else), then the parameter skipContent should be added to prevent it from changing upon saving.

```js
'.landing-block-node-card-button' => array(
    'name' => 'Button',
    'type' => 'link',
    'skipContent' => true
),
```

## icon

Visually similar to an image, as the end user interacts with icon options, with the exception that they cannot upload their own to these nodes. Technically, this is a class of the element that renders a specific icon through standard icon fonts.

```js
'.landing-block-node-list-item-icon' => array(
    'name' => 'Icon',
    'type' => 'icon',
),
```

```js
<i class="landing-block-node-list-item-icon fa fa-instagram"></i> // the class fa-instagram is responsible for displaying the Instagram icon
```

## embed

Multimedia. For example, background video. For this type, only two attributes change – `src` and `source`.

```js
'.landing-block-node-card-videobg' => array(
    'name' => 'Background video',
    'type' => 'embed',
),
```

```js
<iframe
    class="landing-block-node-card-videobg bg-video__video"
    width="100%"
    src="//www.youtube.com/embed/q4d8g9Dn3ww?autoplay=1&controls=0&loop=1&mute=1&rel=0"
    data-source="https://www.youtube.com/watch?v=q4d8g9Dn3ww"
    frameborder="0"
    allowfullscreen="">
</iframe>
```

## map

Allows working with the selector as a geo-map. Currently, only Google Maps are supported. In editing mode, it allows easy management of balloons.

```js
'.landing-block-node-map' => array(
    'name' => 'Location of our office',
    'type' => 'map',
),
```

```js
<div class="landing-block-node-map mx-auto w-100 g-min-height-430 h-100"></div>
```