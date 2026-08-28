# Components and Templates

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 UI Kit is a library of **design-consistent components** that can be combined, extended, and adapted to specific tasks. Its main goal is to speed up development and to keep interfaces consistent across all applications embedded in Bitrix24.

This page helps you choose ready-made elements for an application screen: it shows the categories the library is split into, what each of them holds, and which ready-made project templates exist. Descriptions of individual components, their parameters, and live examples stay in the [library documentation](https://bitrix24.github.io/b24ui/docs/components/).

> Quick links: [Component Categories](#all-components)

## What You Need to Get Started

The library components are Vue components, and they run on Vue 3. There are two ways to start a project: with Nuxt or with Vue and Vite.

If your application is not written in Vue, the components are not suitable. For a different front end, UI Kit provides design tokens as a Tailwind CSS plugin and a library of icons — these are compatible with any stack. Both options are covered in the [{#T}](./quick-start.md) article.

## How to Use a Component

Once the library is connected, a component is called in the template by its name with the `B24` prefix:

```html
<template>
  <B24Button>Button</B24Button>
</template>
```

In the library documentation, component names are given without the prefix: the `Button` section describes the `B24Button` component.

## Component Categories {#all-components}

The library splits components into 13 categories. The table gives a few components from each one; the full contents of a category are available in the library documentation.

#|
|| **Category** | **Purpose** | **Components** ||
|| Layout | the frame of the application page | `B24App`, `B24Container`, `B24Header`, `B24Footer`, `B24Sidebar` ||
|| Element | basic interface elements | `B24Button`, `B24Alert`, `B24Avatar`, `B24Badge`, `B24Card`, `B24Progress` ||
|| Form | input fields and forms | `B24Form`, `B24Input`, `B24Select`, `B24Checkbox`, `B24Switch`, `B24FileUpload` ||
|| Data | data output | `B24Table`, `B24Accordion`, `B24Timeline`, `B24User`, `B24Empty` ||
|| Navigation | moving around the application | `B24Breadcrumb`, `B24Tabs`, `B24Pagination`, `B24Stepper`, `B24Link` ||
|| Overlay | layers on top of the page | `B24Modal`, `B24Slideover`, `B24Drawer`, `B24Popover`, `B24Toast`, `B24Tooltip` ||
|| Page | blocks for assembling a page | `B24Page`, `B24PageHeader`, `B24PageCard`, `B24PageGrid`, `B24PageSection` ||
|| Dashboard | the frame of a panel with a side menu | `B24DashboardPanel`, `B24DashboardSidebar`, `B24DashboardNavbar`, `B24DashboardToolbar` ||
|| AI Chat | the interface of a chat with an assistant | `B24Chat`, `B24ChatMessages`, `B24ChatPrompt`, `B24ChatTool` ||
|| Editor | a text editor | `B24Editor`, `B24EditorToolbar`, `B24EditorMentionMenu` ||
|| Content | search and navigation through materials | `B24ContentSearch`, `B24ContentToc`, `B24ContentSurround` ||
|| Color Mode | switching between the light and dark themes | `B24ColorModeButton`, `B24ColorModeSelect`, `B24ColorModeSwitch` ||
|| i18n | selecting the interface language | `B24LocaleSelect` ||
|#

## Project Templates

Besides individual components, the library provides ready-made project starters. There are two of them, each in a version for Nuxt and for Vue:

- **Starter** — a minimal project with UI Kit connected, suitable for a single-page application or a widget
- **Dashboard** — a project with a side menu, a panel, and a working area, suitable for an application with several sections

The template sources and the launch instructions are available in the [templates section](https://bitrix24.github.io/b24ui/templates/).

## Component Approach

The library components:

- are built on the principle of **composition** — they combine with each other without extra wiring,
- **support theming** — the appearance can be adjusted to the light or dark theme,
- are **responsive** — they look right at different resolutions,
- are **extensible** — through `slots` and `props`.

An interface is assembled from ready-made parts, and there is no need to describe visual styles manually.

## Default Behavior

UI Kit implements the behavior a Bitrix24 user is accustomed to:

- paddings, alignment, and grids — as in the core platform,
- the logic of opening and closing modals, slide-outs, and dropdowns — matching the platform,
- keyboard and focus interaction — following accessibility standards.

You do not need to guess how a component behaves: it is already aligned with the UX patterns of Bitrix24.

## Continue Your Exploration

- [{#T}](./quick-start.md)
- [{#T}](./design.md)
- [{#T}](./app-logic.md)
- [{#T}](./index.md)