# Quick Start

The Bitrix24 UI Kit is not just a component library. It is a **multi-layered system** that allows you to adapt the visual style of Bitrix24 depending on **how deeply you want to integrate** and what stack you are using.

You can use the UI Kit as:

1. A full-fledged **Vue component library**, which we recommend.
2. A source of **design tokens** on Tailwind CSS, along with an **icon library** compatible with any frontend stack.

## Level 1: Full Integration via Vue

If you are using **Vue 3** — you get the most: ready-made components, fully styled in the Bitrix24 design, with support for themes, responsiveness, customization, and UX logic tested on millions of users.

You can choose one of two options to start your project:

- **[Nuxt 3](https://ui3.nuxt.dev/)** — ideal for SSR, multi-page applications, and quick integration with the Bitrix24 infrastructure.
- **Vite** — simplicity, speed, and ease for SPA and microservices architecture.

We support both options. Choose the one that fits your needs.

### Why Nuxt?

- Perfect for applications with navigation, localization, and modular structure.
- SSR/SSG out of the box — great performance.
- Support for middleware, layouts, and composables.

### Why Vite?

- Lightning-fast start and dev server.
- Simple configuration.
- Ideal for embedded widgets, single-page applications, and microfrontend projects.

## Level 2: Design Tokens Only

If you are not using Vue but still want your interface to look **like native Bitrix24** — use our **design token system**, implemented as a [Tailwind CSS](https://tailwindcss.com/docs/utility-first) plugin.

Design tokens include:

- Bitrix24 color palette, including light and dark themes,
- typography,
- spacing, sizes, borders,
- ready-made utilities for Bitrix24 UI patterns.

### Why Tailwind CSS?

- Tailwind CSS provides a powerful and flexible way to **describe the interface through classes**, without the need to write CSS manually.
- Easy integration into any stack: React, Vue, Svelte, even jQuery.
- Ability for rapid prototyping and scaling of components.
- Used by thousands of teams.

{% note info %}

Our plugin is a shortcut for creating interfaces in the Bitrix24 style, even if you are not using our UI Kit on Vue.

{% endnote %}

### Icons

Do you only need icons in the Bitrix24 brand style? Connect `@bitrix24/icons` — and use SVG icons in any framework. All icons are optimized and available through CSS classes or as Vue/React components.

## Starting a Project with Nuxt

The easiest and most correct way to start a project using Nuxt 3 is to use a "template" that includes all the necessary dependencies and settings. A detailed description and example can be found in the [documentation](https://bitrix24.github.io/b24ui/guide/installation-nuxt-app.html).

## Starting a Project with Vite

Similarly to the "template" for Nuxt, you can use the option for Vite. It also includes all the necessary dependencies and settings. A detailed description and example can be found in the [documentation](https://bitrix24.github.io/b24ui/guide/installation-vue.html).