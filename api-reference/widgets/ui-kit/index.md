# Bitrix24 UI Kit: Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Bitrix24 UI Kit is a library of components, design tokens, and icons for Bitrix24 application interfaces. It helps you build an application screen so that it looks and behaves like a native part of Bitrix24.

The library is used for application pages, widgets, and embedded interfaces: forms, lists, buttons, modal windows, and other items. Components do not communicate with Bitrix24 and do not manage user permissions. The application retrieves screen information and access checks separately via Bitrix24 methods, a server-side component, or the SDK.

> Quick links: [UI Kit Page Overview](#all-pages)
>
> Full library documentation: [Bitrix24 UI Kit](https://bitrix24.github.io/b24ui/)

## Getting Started

1. Determine where the user will open the screen: on an application page, in a widget, or in a slider. For a screen inside Bitrix24, select an [embedding location](../placements.md)
2. Connect the UI Kit following the [Quick Start](./quick-start.md) instructions or the documentation for [Nuxt](https://bitrix24.github.io/b24ui/docs/getting-started/installation/nuxt/) and [Vue/Vite](https://bitrix24.github.io/b24ui/docs/getting-started/installation/vue/)
3. Build the screen using Vue components, Tailwind CSS design tokens, or icons. See ready-to-use items in the [Components and Templates](./components.md) article
4. Configure data retrieval via Bitrix24 methods, a server-side component, or the SDK. For more details, see the [Integrating UI Kit with REST API and Business Logic](./app-logic.md) article
5. For a screen in a widget, register a handler using the [placement.bind](../placement-bind.md) method: pass the embedding location code in the `PLACEMENT` parameter and the external application page URL in the `HANDLER` parameter
6. If the handler is registered during application installation, complete the installation following the [Completing Application Installation](../../../settings/app-installation/installation-finish.md) instructions

## Connection with Other Objects

**Application.** The UI Kit is used within the application interface. The installation method and interface availability depend on the [application](../../../settings/app-installation/index.md) settings.

**Widgets.** If a screen is opened from a widget, the UI Kit styles the handler page. Handlers are registered, viewed, and deleted using the [placement.bind](../placement-bind.md), [placement.get](../placement-get.md), and [placement.unbind](../placement-unbind.md) methods.

**Bitrix24 Data.** When a screen requires data, the application calls methods of the Bitrix24 tool that the interface is working with.

**BX24 SDK.** Within a widget, you can open the application interface, navigate to standard Bitrix24 pages, and close the application window using the [Bitrix24 SDK for Widgets](../bx24-widget-methods.md) methods.

## Key Considerations- The UI Kit Does Not Require a Separate Scope: Permissions Are Checked for a Specific Application Action
- To register, view, or delete a handler, check the scope in the [placement.bind](../placement-bind.md), [placement.get](../placement-get.md), and [placement.unbind](../placement-unbind.md) methods
- To retrieve a list of available placement locations, check the scope in the [placement.list](../placement-list.md) method
- For data processing methods, check the scope on the specific method page. See the [Permissions](../../scopes/permissions.md) reference for a complete list of values
- The widget handler must be accessible via an external URL if it is registered using the [placement.bind](../placement-bind.md) method
- Until the application installation is complete, registered widgets are visible only to administrators or users with permission to install applications

## UI Kit Page Overview {#all-pages}

#|
|| **If you need to** | **Open the page** ||
|| Connect UI Kit to a project | [Quick start](./quick-start.md) ||
|| Understand Bitrix24 interface visual rules | [Design principles](./design.md) ||
|| Select ready-made items for an app screen | [Components and templates](./components.md) ||
|| Connect the interface with app data and logic | [UI Kit integration with REST API and business logic](./app-logic.md) ||
|#
