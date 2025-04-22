# Components and Templates

The Bitrix24 UI Kit is a library of **design-consistent components** that can be combined, extended, and adapted for specific tasks. The main goal is to accelerate development and ensure uniformity of interfaces across all applications integrated into Bitrix24.

## Categories of Components

The UI Kit components are grouped into meaningful categories:

- **Actions** — buttons, dropdowns, modals, sliders
- **Notifications** — alerts, toasts, tooltips, progress bars
- **Information Display** — avatars, badges, icons, calendars, tables
- **Data Input** — inputs, selectors, checkboxes, radio buttons
- **Navigation** — menus, tabs, links

You can find the complete list of available components in the [detailed documentation](https://bitrix24.github.io/b24ui/components/app.html).

### Component Approach

All components:

- are built on the principle of **composition** — easily combine with each other,
- **work with theming** — external styling can be adjusted for light/dark themes,
- are **responsive** — look good on different resolutions,
- are **extensible** — through `slots` and `props`.

You can assemble interfaces from ready-made parts without the need to manually describe visual styles.

## Default Behavior

The UI Kit implements the behavior that Bitrix24 users are accustomed to:

- margins, alignment, grids — as in the main platform,
- logic for opening/closing modals, dropdowns, and pop-ups — identical to the platform's,
- keyboard interaction and focus — according to accessibility standards.

This eliminates the need to "guess" how a component should behave — everything is already aligned with Bitrix24's UX patterns.