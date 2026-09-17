# Forms in Blocks

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A block with a form embeds a Bitrix24 CRM form into a site page: a request, a callback order, or a subscription. The data from such a form goes to CRM instead of being retained on the page.

The scenario suits pages that collect requests: a promotion landing page, a contacts page, or a request form in an online store. If the data does not have to reach CRM, use regular block nodes.

Embedding is enabled through `subtype: form` in the `block` section of the [block manifest](../manifest.md). The subtype prepares the block and adds the form settings to the editor. The form itself is created and configured in CRM — the block only selects a ready-made form and defines its design. The list of forms can be opened directly from the block settings in the site editor.

## Requirements for Block Functionality

In the block manifest, specify `subtype: form`, and in the markup, add the node `.bitrix24forms` — the subtype does not recognize any other selector.

Minimal example:

```php
'block' => [
    'subtype' => 'form',
],
'assets' => [
    'ext' => [
        'landing_form',
    ],
],
```

```html
<div class="bitrix24forms" data-b24form-use-style="Y"></div>
```

In standard blocks, the `landing_form` extension is specified in the manifest. If it is not present, the subtype will add it automatically.

## How the Subtype Works

The subtype operates in two phases.

**Building the manifest.** When the system builds the block manifest, the subtype handler connects the `landing_form` extension, adds the `crm-form` style setting for `.bitrix24forms`, and describes the form attributes. In the editor, these attributes turn into the block settings: the list of available forms and the link to the forms page.

**Adding the block to a page.** In the `afterAdd` callback, the system checks the `.bitrix24forms` node:

- if the markup does not contain `.bitrix24forms`, automatic configuration is not performed
- if a form is already selected for the block, the system retains it
- if no form is selected yet, the system substitutes a ready-made form and creates a new one if necessary
- the selected form is recorded in the `data-b24form` attribute as a marker of the `#crmFormInline<ID>` form
- the content of the node is replaced with a preloader

The marker and the preloader stay in the block content. The system replaces them with the working embedding code when the page is rendered.

The `#crmFormInline` prefix means that the form is embedded into the page. A form can also be opened in a popup, but that is a link rather than a block with a form: a marker of the `#crmFormPopup<ID>` form is recorded in the link attribute of the button.

## Form Attributes

#|
|| **Attribute** | **Value** | **What It Defines** ||
|| `data-b24form` | A marker of the `#crmFormInline<ID>` form, where `ID` is the identifier of the CRM form | The selected form ||
|| `data-b24form-use-style` | `Y` or `N` | Whether the design defined in the block is used ||
|| `data-b24form-design` | JSON, the set of keys is listed below | The design of the form ||
|#

The subtype sets two more attributes itself, and they are not shown in the interface: `data-b24form-embed` — a flag of an embedded form, and `data-b24form-connector` with the value `Y` — the form is connected without the CRM module.

Keys of `data-b24form-design`:

#|
|| **Key** | **Type** | **What It Defines** ||
|| `dark` | Boolean | The dark theme of the form ||
|| `style` | String | The design style, `classic` for example ||
|| `shadow` | Boolean | The shadow around the form ||
|| `compact` | Boolean | The compact mode of the fields ||
|| `color` | Object | The colors of the form elements ||
|| `border` | Object with the keys `top`, `bottom`, `left`, `right` | The visible sides of the frame ||
|#

Example value:

```json
{
    "dark": true,
    "style": "classic",
    "shadow": false,
    "compact": false,
    "color": {},
    "border": {
        "top": false,
        "bottom": false,
        "left": false,
        "right": false
    }
}
```

The `.landing-block-form-styles` element and the `data-b24form-show-header` attribute relate to the migration of the old format and are not needed in new blocks.

## How to Change the Form via REST

1. Retrieve the block manifest using the [landing.block.getmanifest](../methods/landing-block-get-manifest.md) method with the `params.edit_mode = true` parameter and review which values are available for `.bitrix24forms` in the `attrs` key. The list contains the forms available in Bitrix24. Without `edit_mode`, the method returns only the name of the `data-b24form` attribute, with no list of forms.
2. Record the new `data-b24form` value using the [landing.block.updateattrs](../methods/landing-block-update-attrs.md) method.
3. Check the result using the [landing.block.getcontent](../methods/landing-block-get-content.md) method with the `editMode = true` parameter — without it, the published version of the block is returned. Then publish the page using the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method.

Your own block with a form is registered using the [landing.repo.register](../../user-blocks/landing-repo-register.md) method, and the manifest of a standard block can be retrieved as a sample using the [landing.block.getmanifestfile](../methods/landing-block-get-manifest-file.md) method.

## Examples of Standard Blocks

- `33.1.form_1_transparent_black_left_text`
- `33.10.form_2_light_left_text`
- `33.23.form_2_themecolor_no_text`
- `66.90.form_new_default`

## Permissions and Limitations

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

No additional scope is required for embedding a form: the block works with the `landing` scope, and the set of available forms is determined by which CRM forms exist in Bitrix24.

Limitations:

- the block embeds a ready-made CRM form. The form itself cannot be created or modified with the `landing.block.*` methods
- the subtype processes only the `.bitrix24forms` node, it does not recognize any other selector
- the list of forms in the block settings is built from the forms available in Bitrix24. If there are no available forms, there will be nothing to choose from
- if a block has several subtypes, all of them extend the manifest, but the `afterAdd` callback runs only for the last one in the list. In a block with `subtype: ['map', 'form']`, the automatic map setup will not work

## Continue Your Learning

- [{#T}](./index.md)
- [{#T}](./menu.md)
- [{#T}](./maps.md)
- [{#T}](./navigation.md)
- [{#T}](./search.md)
- [{#T}](./search-forms.md)