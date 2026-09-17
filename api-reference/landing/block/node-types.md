# Node Types

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A node is an editable element of a block: a heading, an image, a button, an icon, a video, or a map. Nodes are described in the `nodes` key of the [block manifest](./manifest.md): each one is tied to a CSS selector, while its type determines which form the element is edited with in the interface and in which format the value is retained.

The node type is needed in two cases: when you build your own block and pass the manifest using the [landing.repo.register](../user-blocks/landing-repo-register.md) method, and when you modify a block via REST — the selector and the value format in [landing.block.updatenodes](./methods/landing-block-update-nodes.md) depend on the node type.

Nodes describe only the editable elements. The design of a block is defined by the `style` key, additional settings by the [attrs](./attributes.md) key, and repeatable elements by the `cards` key.

After reading this article, you will be able to describe the nodes in the manifest of your own block and choose the value format for the [landing.block.updatenodes](./methods/landing-block-update-nodes.md) method.

## What Node Types Exist

#|
|| **Type** | **What It Edits** | **Typical Markup** ||
|| `text` | The text of a heading, a paragraph, or a caption | `<h2>`, `<div>`, `<span>` ||
|| `img` | An image or the background of a container. Use it when the image is changed as content | `<img>`, `<div>` with `background-image` ||
|| `link` | The address of a link and the related parameters | `<a>` ||
|| `icon` | The CSS class of an icon | `<i>` ||
|| `embed` | Embedded media from an external service, a YouTube video for example | `<iframe>` ||
|| `map` | A map with a center, a zoom level, and markers | `<div>` with the `data-map` attribute ||
|| `component` | A built-in Bitrix24 component, a product catalog for example | An empty `<div>` container ||
|| `styleimg` | An image controlled by the design settings. Use it for backgrounds and covers set in the design form | An empty `<div>` container ||
|#

## Node Fields

A node has common fields that are used across different types and set the basic configuration of the element in the editor. In addition to these, there are type-dependent fields that work only for specific types.

Common fields:

- `name` — the name of the node in the editing interface
- `type` — the type of the node
- `allowInlineEdit` — controls the availability of the node for inline editing. If set to `false`, the node will be unavailable for inline editing but will remain accessible in the block editing form
- `useInDesigner` — controls the participation of the node in the block designer. If set to `false`, the element will be ignored in the block designer
- `group` — grouping of nodes. If multiple nodes in one block are assigned the same value, clicking on any of them will open a common editing form for the group

Fields for specific types:

#|
|| **Field** | **For Which Types** | **What It Defines** ||
|| `dimensions` | `img` | The size restrictions of the uploaded image ||
|| `create2xByDefault` | `img` | The creation of a `2x` version of the image by default ||
|| `skipContent` | `link` | Retaining the internal content of the link unchanged ||
|| `extra` | `component` | The description of the editable parameters of the component ||
|#

The set of fields depends on the type of node and the specific scenario of the block.

## Type Descriptions

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

Image node. A node can be a separate `<img>` tag or a background image of a container, such as `<div>`.

For this type, it is recommended to set `dimensions` to control the size of uploaded images and avoid retaining excessively large files in Bitrix24.

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

The value of such a node is an object. Its keys are passed in the [landing.block.updatenodes](./methods/landing-block-update-nodes.md) method, and the system distributes them across the attributes of the element:

- `src` — the address of the embedded content. For `<iframe>`, it is saved in the `src` attribute; for other embedding options, `data-src` may be used
- `source` — the original URL, saved in the `data-source` attribute
- `preview` — the URL of the preview image, saved in `data-preview`
- `ratio` — the aspect ratio of the container. The allowed values are `embed-responsive-16by9`, `embed-responsive-9by16`, `embed-responsive-4by3`, `embed-responsive-3by4`, `embed-responsive-21by9`, `embed-responsive-9by21`, `embed-responsive-1by1`. The value is applied only if a new `src` is passed in the same call and the parent of the node has the `embed-responsive` class

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

The provider is set by the `data-map-provider` attribute, and the values `google` and `yandex` are supported. How the provider is selected when a block is added is described in the [Maps in Blocks](./special/maps.md) article.

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
        "center":{"lat":52.520008,"lng":13.404954},
        "zoom":12,
        "markers":[
            {
                "title":"Office",
                "description":"Berlin, center",
                "showByDefault":true,
                "latLng":{"lat":52.520008,"lng":13.404954}
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

## How to Change a Node via REST

The node selectors from the manifest are used by the block modification methods:

- the content of a node is changed by [landing.block.updatenodes](./methods/landing-block-update-nodes.md). The key in the `data` parameter is the node selector, and the format of the value depends on the node type. The formats for each type are listed in the [Value Formats in data](./methods/landing-block-update-nodes.md#value-formats) section
- the tag name of a node is changed by [landing.block.changeNodeName](./methods/landing-block-change-node-name.md), `h2` to `h3` for example
- an image for a node of the `img` type is first uploaded using the [landing.block.uploadfile](./methods/landing-block-upload-file.md) method, and then the resulting `src` is substituted using the `landing.block.updatenodes` method

To find out which nodes a specific block has, use the [landing.block.getmanifest](./methods/landing-block-get-manifest.md) method for a block placed on a page or [landing.block.getmanifestfile](./methods/landing-block-get-manifest-file.md) for a template from the repository.

## Permissions and Limitations

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

Limitations:

- only the elements described in the `nodes` key can be modified. Selectors absent from the manifest are ignored by the [landing.block.updatenodes](./methods/landing-block-update-nodes.md) method
- it is better not to make a node selector identical to a card selector of the same block: the system does not prohibit this, but in the editor it becomes unclear what is being edited
- a selector from `nodes` must not have a background type of `background`, `block-default`, or `block-border` in `style`: [landing.repo.register](../user-blocks/landing-repo-register.md) returns the `MANIFEST_INTERSECT_IMG` error
- a custom set of nodes is described only in your own block, which is registered using the [landing.repo.register](../user-blocks/landing-repo-register.md) method. The manifest of a standard Bitrix24 block cannot be modified via REST, see [Block Manifest](./manifest.md) for details

## Continue Your Learning

- [{#T}](./manifest.md)
- [{#T}](./attributes.md)
- [{#T}](./extended-description.md)
- [{#T}](./localization.md)
- [{#T}](./methods/landing-block-update-nodes.md)
- [{#T}](./methods/landing-block-get-manifest.md)