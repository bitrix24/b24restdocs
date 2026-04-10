# Forms in Blocks

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Forms in blocks operate through `subtype: form`. This subtype prepares the block for embedding CRM forms and adds form settings in the editor.

## Requirements for Block Functionality

In the block manifest, specify `subtype: form`, and in the markup, add the node `.bitrix24forms`.

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

## How the Subtype Configures the Block

When preparing the manifest, the subtype:

- connects the `landing_form` extension
- adds the style setting `crm-form` for `.bitrix24forms`
- adds form settings for the node `.bitrix24forms` in the editor

In Bitrix24, a link to the forms page appears in the block settings.

In the public part and during publication, the system replaces the form marker with the working embedding code.

## What Happens After Adding the Block

After adding the block, the system checks the node `.bitrix24forms`.

- if the markup does not contain `.bitrix24forms`, automatic configuration will not be executed
- if a form is already selected for the block, the system will retain it
- if no form is selected yet, the system will attempt to substitute a ready-made form and will create a new one if necessary
- after this, the system records the selected form in the `data-b24form` attribute

Inside the block, the form is saved as a marker of the type `#crmFormInline<ID>`.

## What to Check Before Use

- the node `.bitrix24forms` must already be in the block markup
- form styles in current standard blocks are set through `data-b24form-design` and `data-b24form-use-style`
- the set of settings in the editor depends on whether there are available forms in Bitrix24
- old elements `.landing-block-form-styles` and the attribute `data-b24form-show-header` relate to the migration of the old format and are not needed for new blocks
- the `form` subtype can also be used in combined blocks, for example, alongside `map`

## Examples of Standard Blocks

- `33.1.form_1_transparent_black_left_text`
- `33.10.form_2_light_left_text`
- `33.23.form_2_themecolor_no_text`
- `66.90.form_new_default`

To ensure the form functions correctly, add the node `.bitrix24forms` and specify `subtype: form` in the manifest.

## Continue Your Learning

- [Special Blocks](./index.md)
- [Menu Blocks](./menu.md)
- [Maps in Blocks](./maps.md)
- [Navigation and Header](./navigation.md)
- [Search Results](./search.md)
- [Search Forms](./search-forms.md)