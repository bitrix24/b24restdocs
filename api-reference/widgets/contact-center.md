# Widget in the CONTACT_CENTER

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`contact_center`](../scopes/permissions.md)

The CONTACT_CENTER placement adds an item to the Contact Center application list.

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](./placement-bind.md) method.

{% note info "" %}

The widget will not be displayed in the interface until the application installation is complete. [Check the application installation](../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the widget is embedded

#|
|| **Widget Code** | **Location** ||
|| `CONTACT_CENTER` | Item in the Contact Center list ||
|#

### Where to find it in the interface

Open the Contact Center page at `https://your_site.com/contact_center/`. The application item with `PLACEMENT=CONTACT_CENTER` appears at the bottom of the page in the *Partner Solutions* section.

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```php

Array
(
    [DOMAIN] => example.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => de
    [APP_SID] => 0123456789abcdef0123456789abcdef
    [APPLICATION_SCOPE] => crm,placement,contact_center,imopenlines
    [APPLICATION_TOKEN] => xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    [AUTH_ID] => 6061e72600631fcd00005a4b00000001f0f1076700000000f69dd5fc643d9ce2fdbc1
    [AUTH_EXPIRES] => 3600
    [PLACEMENT_OPTIONS] => {"ID":"19"}
    [REFRESH_ID] => 50e00aa340631fcd00005a4b00000001f0f1071111116580a5b83c2de639ef28c12
    [SERVER_ENDPOINT] => https://oauth.bitrix24.info/rest/
    [member_id] => abcdef1234567890abcdef1234567890
    [status] => F
    [PLACEMENT] => CONTACT_CENTER
)

```

{% include notitle [Footnote on required parameters](../../_includes/required.md) %}

{% include notitle [description of standard data](_includes/widget_data.md) %}

### Additional Data

#|
|| **Parameter**
`type` | **Description** ||
|| **APPLICATION_SCOPE**
[`string`](../data-types.md) | List of scopes available to the application ||
|| **APPLICATION_TOKEN**
[`string`](../data-types.md) | Application token for secure event handling ||
|| **SERVER_ENDPOINT**
[`string`](../data-types.md) | Bitrix24 authorization server address required for updating OAuth 2.0 tokens ||
|#

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the context of the call.

For `CONTACT_CENTER`, the context includes the key:

- `ID` — identifier of the Contact Center entity for which the widget was opened

## Continue your exploration

- [{#T}](./placement-bind.md)
- [{#T}](./ui-interaction/index.md)
- [{#T}](../../settings/interactivity/index.md)
- [{#T}](./bx24-widget-methods.md)