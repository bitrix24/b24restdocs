# Node Types

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Nodes are described in the `nodes` key of the [block manifest](./manifest.md). Each node is tied to a CSS selector and defines how a specific element of the block is edited in the interface.

## Node Fields

A node has common fields that are used across different types and set the basic configuration of the element in the editor. In addition to these, there are type-dependent fields that work only for specific types.

Common fields:

- `name` — the name of the node in the editing interface
- `type` — the type of the node
- `allowInlineEdit` — controls the availability of the node for inline editing. If set to `false`, the node will be unavailable for inline editing but will remain accessible in the block editing form
- `useInDesigner` — controls the participation of the node in the block designer. If set to `false`, the element will be ignored in the block designer
- `group` — grouping of nodes. If multiple nodes in one block are assigned the same value, clicking on any of them will open a common editing form for the group

Fields for specific types:

- `dimensions` — image size parameters for `img`
- `create2xByDefault` — enable the creation of a `2x` version by default for `img`
- `skipContent` — do not modify internal content when saving for `link`
- `extra` — description of editable parameters for the component for `component`

The set of fields depends on the type of node and the specific scenario of the block.

## Node Types

### text

Text node for headings, paragraphs, and other text elements.

```php
'.landing-block-node-card-title' => [
    'name' => 'Heading',
    'type' => 'text',
],
```

```html
<h2 class="landing-block-node-card-title">Company24 video</h2>
```

### img

Image node. It can be a separate `<img>` tag or a background image for a container, such as `<div>`.

For this type, it is recommended to set `dimensions` to control the size of uploaded images and avoid overloading the account with excessively large files.

Supported options for `dimensions`:

- `width` / `height` — set to a fixed size
- `maxWidth` / `maxHeight` — reduce if the image exceeds the specified size
- `minWidth` / `minHeight` — increase until the minimum is reached

```php
'.landing-block-node-card-image' => [
    'name' => 'Image',
    'type' => 'img',
    'dimensions' => [
        'maxWidth' => 1920,
        'maxHeight' => 1080,
    ],
],
```

```html
<img class="landing-block-node-card-image" src="/upload/demo.jpg" alt="">
```

To display the image, the node must have an image source specified:
- for the `<img>` tag, the `src` attribute is used
- for a background element, such as `<div>`, the CSS property `background-image` is used

### link

Link node. Allows editing the address and related parameters of the link, such as link text or opening mode.

```php
'.landing-block-node-card-button' => [
    'name' => 'Button',
    'type' => 'link',
],
```

```html
<a class="landing-block-node-card-button btn btn-primary" href="/">Read more</a>
```

If the link wraps non-text content, you can specify `skipContent => true` to avoid modifying the internal content when saving:

```php
'.landing-block-node-card-button' => [
    'name' => 'Button',
    'type' => 'link',
    'skipContent' => true,
],
```

### icon

Icon node. Typically changes the CSS class that defines the displayed icon.

```php
'.landing-block-node-list-item-icon' => [
    'name' => 'Icon',
    'type' => 'icon',
],
```

```html
<i class="landing-block-node-list-item-icon fa fa-check"></i>
```

### embed

Node for embedding media content, such as video.

In a typical scenario for this node, the following fields are used:
- `src` — the address of the embedded content. For `<iframe>`, it is saved in the `src` attribute; for other embedding options, `data-src` may be used
- `source` — the original URL, saved in the `data-source` attribute
- `preview` — the URL of the preview image, saved in `data-preview`
- `ratio` — the aspect ratio of the container. A utility CSS class of the form `embed-responsive-*` is used

```php
'.landing-block-node-video' => [
    'name' => 'Video',
    'type' => 'embed',
],
```

```html
<div class="embed-responsive embed-responsive-16by9">
    <iframe
        class="landing-block-node-video"
        width="100%"
        src="//www.youtube.com/embed/q4d8g9Dn3ww"
        data-source="https://www.youtube.com/watch?v=q4d8g9Dn3ww"
        data-preview="https://example.com/preview.jpg"
        frameborder="0"
        allowfullscreen>
    </iframe>
</div>
```

### map

Map node for blocks with geographical references.

In the current implementation, `google` provider is used. If the provider is not explicitly specified, `google` is used by default.

The typical structure of the map value in `data-map` includes:
- `center` — coordinates of the center of the map
- `zoom` — zoom level
- `markers` — an array of markers

```php
'.landing-block-node-map' => [
    'name' => 'Map',
    'type' => 'map',
],
```

```html
<div
    class="landing-block-node-map"
    data-map-provider="google"
    data-map='{
        "center":{"lat":55.751244,"lng":37.618423},
        "zoom":12,
        "markers":[
            {
                "title":"Office",
                "description":"New York, center",
                "showByDefault":true,
                "latLng":{"lat":55.751244,"lng":37.618423}
            }
        ]
    }'>
</div>
```

### component

Node for embedding a component into the block structure.

```php
'.landing-block-node-catalog' => [
    'name' => 'Catalog',
    'type' => 'component',
],
```

```html
<div class="landing-block-node-catalog"></div>
```

### styleimg

Image node managed through the style settings of the block.

```php
'.landing-block-node-cover' => [
    'name' => 'Background Image',
    'type' => 'styleimg',
],
```

```html
<div class="landing-block-node-cover"></div>
```

## Node Grouping

To have multiple nodes open in a single editing form, specify the same `group` value.

```php
'.landing-block-node-title' => [
    'name' => 'Heading',
    'type' => 'text',
    'group' => 'hero-content',
],
'.landing-block-node-text' => [
    'name' => 'Text',
    'type' => 'text',
    'group' => 'hero-content',
],
'.landing-block-node-button' => [
    'name' => 'Button',
    'type' => 'link',
    // without group: edited separately
],
```