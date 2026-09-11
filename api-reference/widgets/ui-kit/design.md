# Principles of the Bitrix24 UI Kit Design

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Bitrix24 UI Kit is built so that an application screen looks and behaves like a part of Bitrix24, not like an external site in a frame. The six principles below explain what creates this resemblance and how the library delivers each of them: with a component, ready-made styling, or default behavior.

For setup instructions, see [{#T}](./quick-start.md); for choosing ready-made elements, see [{#T}](./components.md). Component parameters and interactive examples are available in the [library documentation](https://bitrix24.github.io/b24ui/).

## 1. Consistency

Interface patterns should be recognizable and predictable for Bitrix24 users. This means:

- identical spacing, sizes, and grids
- consistent interaction patterns
- unified colors and typography

You do not need to reproduce these patterns manually. Spacing, alignment, and grids in the components follow the Bitrix24 interface, and modal windows, slideovers, and dropdown lists open and close the same way as in the rest of the product. The ready-made elements are listed in the [{#T}](./components.md) article.

## 2. Adaptability and Scalability

Components function correctly on different screens—from laptops to embedded panels in mobile applications.

- The width of a component adapts to its container, with no separate layout for each screen.
- Responsiveness is set with Tailwind CSS classes directly in the template, without manual media queries.
- Sizes and spacing come from a shared set of design tokens rather than being defined anew on every screen.

Design tokens are ready-made styling values: the Bitrix24 palette with light and dark themes, typography, spacing, sizes, and borders. They ship with the library and are available in the markup as Tailwind CSS classes, so there is no need to write CSS by hand. Learn more in the [{#T}](./quick-start.md) article.

## 3. Inclusivity and Accessibility

The components are built on Reka UI, a library of unstyled components with built-in support for assistive technologies and keyboard interaction. It follows the WAI-ARIA patterns — the W3C recommendations for accessible web interfaces. This provides:

- correct semantics and element roles
- keyboard navigation in composite components, including tabs
- closing modal windows and slideovers with the Escape key
- isolation of dialog content from the rest of the page for screen readers
- dark theme compatibility

This does not cover the accessibility of the whole screen: heading order, label texts, and clear hints are the responsibility of your code.

Switching between light and dark themes is handled by the components of the Color Mode category — they are listed in the [{#T}](./components.md) article.

## 4. Simplicity

The less clutter there is, the clearer it is to the user what to do next. A screen is made simple by:

- a minimum of extraneous information
- a logical hierarchy
- default values that already match Bitrix24

You do not need to configure a component's appearance before using it for the first time. You can start a project from a ready-made template with the library already connected. The templates are listed in the [{#T}](./quick-start.md) article.

## 5. Component-Based

The library is built as a set of Vue components that can be used individually or assembled into templates.

- A component can be placed on a screen independently of other library elements.
- Component behavior is configured with `props`, and nested content is passed through `slots`.
- Components combine with each other without additional wiring.

In a template, a component is called by its name with the `B24` prefix. The full component catalog, organized by category, is in the [{#T}](./components.md) article.

## 6. Brand Identity

The UI Kit follows the Bitrix24 visual language: colors, typography, button shapes, icons, and notification logic match the styling of the rest of the product.

The palette and typography come with the library's design tokens, and the icons ship as a separate package, `@bitrix24/b24icons-vue`. Connecting tokens and icons is described in the [{#T}](./quick-start.md) article.

## What Is Handled for You and What Remains on Your Side

The library itself provides five of the six principles: consistency, adaptability, accessibility, the component-based approach, and brand identity come with every ready-made component. Simplicity is up to you — the library does not decide how many elements and how much information to place on a screen.

The whole application must be wrapped in the `B24App` component. Without this wrapper, notifications, hints, and modal windows opened from code do not work. The connection procedure is described in the [{#T}](./quick-start.md) article.

The library works only in front-end applications built with Vue 3.5 or later: the components are written in Vue, and the design tokens are compiled as part of the project rather than loaded separately.

The UI Kit is responsible only for the interface. The application retrieves screen data and checks user permissions separately — through Bitrix24 methods, the server side, or an SDK. This is described in the [{#T}](./app-logic.md) article.

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./quick-start.md)
- [{#T}](./components.md)
- [{#T}](./app-logic.md)