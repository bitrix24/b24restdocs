# Quick Start: Connecting the UI Kit to a Project

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Bitrix24 UI Kit is connected to the application frontend as the npm package `@bitrix24/b24ui-nuxt`. Once it is connected, the project has access to Vue components, design tokens, and Tailwind CSS utilities in the Bitrix24 style.

The setup differs for a Nuxt project and for a Vue project with Vite: the set of packages and the configuration files are not the same. Parameters of individual components and interactive examples remain in the [library documentation](https://bitrix24.github.io/b24ui/).

## What You Need Before You Start

Requirements of the `@bitrix24/b24ui-nuxt` package:

- Node.js version 20.19 or later in the 20 branch, or 22.12 and later
- Vue version 3.5 or later, and 3.5.24 or later for the icon package
- [Tailwind CSS](https://tailwindcss.com/docs/utility-first) version 4 — the library styles are imported into the project CSS file and use the directives of the fourth version
- Nuxt version 4.1 or later, if the project is built with Nuxt
- a package manager: npm, pnpm, yarn, or bun

The commands on this page are given for npm. For a different package manager, replace them with the equivalents: `pnpm add`, `yarn add`, or `bun add` instead of `npm install`.

An authorization key and a scope are not required to connect the library. The UI Kit is responsible for the interface only and does not access Bitrix24. The application retrieves data and permissions separately — this is described in the [Integration of UI Kit with REST API and Business Logic](./app-logic.md) article.

## How to Choose a Setup Option

#|
|| **What You Have** | **Where to Start** ||
|| No project yet | Clone a [project starter](#starter): the dependencies and settings are already in place ||
|| A project on Nuxt 4.1 or later | [Connecting the Library to a Nuxt Project](#nuxt) ||
|| A project on Vue 3.5 or later with Vite | [Connecting the Library to a Vue and Vite Project](#vite) ||
|| A frontend not based on Vue 3.5 or later | The library cannot be connected: the components are written in Vue, and the [design tokens](#tokens) are built together with the project ||
|#

Nuxt and Vite are supported equally — the choice depends on the task:

- **Nuxt** suits an application with several pages, navigation, and localization. It provides server-side rendering, middleware, layouts, and composables out of the box.
- **Vite** suits a single-page application and a widget that opens in a Bitrix24 slider. Its configuration is shorter, and the dev server starts faster.

## Project Starter {#starter}

A starter is a ready-made project with the UI Kit: the dependencies are already listed, the bundler is configured, and the library styles are connected. There are two starters, they differ only in the bundler — choose one by the same criteria as the setup option.

A starter for a Nuxt project:

```bash
git clone https://github.com/bitrix24/starter-b24ui.git my-app
cd my-app
npm install
npm run dev
```

A starter for a Vue and Vite project:

```bash
git clone https://github.com/bitrix24/starter-b24ui-vue.git my-app
cd my-app
npm install
npm run dev
```

Once the project starts, open the dev server address. To make sure the library styles are applied, see the [How to Verify the Setup](#check) section.

Besides the minimal starters, there are Dashboard starters — projects with a side menu, a panel, and a working area: one for Nuxt and one for Vue. Their contents are described in the [Components and Templates](./components.md) article.

## Connecting the Library to a Nuxt Project {#nuxt}

1. Install the packages:

   ```bash
   npm install @bitrix24/b24ui-nuxt @bitrix24/b24icons-vue tailwindcss
   ```

   `@bitrix24/b24ui-nuxt` is the library itself, and `@bitrix24/b24icons-vue` is a separate package with [icons](#icons). If the project does not need icons, you can skip this package. Tailwind CSS is handled by the library module, so a separate Tailwind CSS plugin is not required. The same steps with examples are available in the [installation instructions for Nuxt](https://bitrix24.github.io/b24ui/docs/getting-started/installation/nuxt/).

2. Connect the module and the project CSS file in `nuxt.config.ts`:

   ```ts
   export default defineNuxtConfig({
     modules: ['@bitrix24/b24ui-nuxt'],
     css: ['~/assets/css/main.css']
   })
   ```

3. Create the `app/assets/css/main.css` file and import Tailwind CSS and the library styles into it:

   ```css
   @import "tailwindcss";
   @import "@bitrix24/b24ui-nuxt";
   ```

   The paths in steps 2 and 3 point to the same file: in Nuxt 4, the `~` alias leads to the `app` directory.

4. Wrap the application in the `B24App` component in the `app.vue` file:

   ```html
   <template>
     <B24App>
       <NuxtPage />
     </B24App>
   </template>
   ```

   `B24App` is the root component of the library. It sets the common application settings and the containers where notifications, tooltips, and windows above the page are rendered. Without this wrapper, `B24Toast` notifications, `B24Tooltip` tooltips, and modal windows opened from code via `useOverlay` do not work. Localization in this case runs with the default values, and the language cannot be switched.

The module registers the components automatically — you do not need to import them on pages.

## Connecting the Library to a Vue and Vite Project {#vite}

1. Install the packages:

   ```bash
   npm install @bitrix24/b24ui-nuxt @bitrix24/b24icons-vue tailwindcss vue-router
   ```

   The `@bitrix24/b24ui-nuxt` package is the same for both setup options. In a Vite project, it provides the build plugin `@bitrix24/b24ui-nuxt/vite` and the Vue plugin `@bitrix24/b24ui-nuxt/vue-plugin`, while the Nuxt module is not used. The build plugin prepares the library components and styles, and the Vue plugin registers them in the application.

   `vue-router` is installed separately for Vite: it is optional in the library dependencies, but by default the plugin renders component links through `RouterLink`. You can turn this behavior off with the `router: false` option in `vite.config.ts` — the links then become regular `a` tags, and the router is not needed. For a Laravel or AdonisJS project with Inertia, set the value `router: 'inertia'`. The setup procedure is described in the [installation instructions](https://bitrix24.github.io/b24ui/docs/getting-started/installation/vue/).

2. Add the build plugin to `vite.config.ts`:

   ```ts
   import { defineConfig } from 'vite'
   import vue from '@vitejs/plugin-vue'
   import bitrix24UIPluginVite from '@bitrix24/b24ui-nuxt/vite'

   export default defineConfig({
     plugins: [
       vue(),
       bitrix24UIPluginVite()
     ]
   })
   ```

   You do not need to add a separate Tailwind CSS plugin to the configuration: Tailwind CSS is handled by `bitrix24UIPluginVite`.

3. Create the `src/assets/css/main.css` file and import Tailwind CSS and the library styles into it:

   ```css
   @import "tailwindcss";
   @import "@bitrix24/b24ui-nuxt";
   ```

4. Connect the project CSS file and the Vue plugin in `src/main.ts`:

   ```ts
   import './assets/css/main.css'
   import { createApp } from 'vue'
   import { createRouter, createWebHistory } from 'vue-router'
   import b24UiPlugin from '@bitrix24/b24ui-nuxt/vue-plugin'
   import App from './App.vue'

   const app = createApp(App)
   const router = createRouter({
     routes: [],
     history: createWebHistory()
   })

   app.use(router)
   app.use(b24UiPlugin)
   app.mount('#app')
   ```

5. Wrap the application in the `B24App` component in the `src/App.vue` file:

   ```html
   <template>
     <B24App>
       <RouterView />
     </B24App>
   </template>
   ```

6. Add the `isolate` class to the root container in `index.html`. The class creates a separate stacking context so that the `z-index` of modal windows and pop-up menus from the library does not conflict with the rest of the page markup:

   ```html
   <div id="app" class="isolate"></div>
   ```

7. Add the type files `components.d.ts` and `auto-imports.d.ts` to `include` in `tsconfig.app.json` and to `.gitignore`. The build plugin registers components and composables automatically, the same way the module does in Nuxt, and creates these files on its own:

   ```json
   {
     "include": ["src/**/*.ts", "src/**/*.tsx", "src/**/*.vue", "auto-imports.d.ts", "components.d.ts"]
   }
   ```

8. In `paths` of the `tsconfig.app.json` and `tsconfig.node.json` files, specify the alias of the directory created by the plugin. Without the alias, theme autocompletion does not work:

   ```json
   {
     "compilerOptions": {
       "paths": {
         "#build/b24ui": ["./node_modules/.b24ui-nuxt/b24ui"]
       }
     }
   }
   ```

The type files appear only after Vite has been started. If the type check runs in the build script before the build itself, it fails on a clean clone: start the dev server or the build first, and run the type check after that.

## How to Verify the Setup {#check}

Add a button to an application page:

```html
<template>
  <B24Button>Button</B24Button>
</template>
```

Run the project with the `npm run dev` command and open the page. If the library is connected, the button is rendered in the Bitrix24 style.

If the button looks like a regular browser button, the styles are not connected. Check the `@bitrix24/b24ui-nuxt` import in the project CSS file and the path to this file: in a Nuxt project, in `nuxt.config.ts`, and in a Vite project, in `src/main.ts`.

## Design Tokens and Icons {#tokens}

### Design Tokens

The tokens are part of the library: they are connected by the `@import "@bitrix24/b24ui-nuxt"` line in the project CSS file. The tokens define the Bitrix24 color palette with a light and a dark theme, typography, spacing, sizes, and borders. In markup, they are available as Tailwind CSS classes, so you do not need to write CSS manually. You can override the values with the `@theme` directive in the same CSS file:

```css
@import "tailwindcss";
@import "@bitrix24/b24ui-nuxt";

@theme {
  --breakpoint-3xl: 1920px;
}
```

The tokens cannot be connected separately from the project build: the library styles are built by the Nuxt module or by the Vite plugin. The library documentation does not describe a way to connect the tokens alone without Vue — the installation is covered for Nuxt and for Vue with Vite.

### Icons {#icons}

The icons ship as a separate package, `@bitrix24/b24icons-vue`. These are Vue components, and each icon is imported from a separate file:

```html
<script setup>
import RocketIcon from '@bitrix24/b24icons-vue/main/RocketIcon'
</script>

<template>
  <B24Button :icon="RocketIcon">Button</B24Button>
</template>
```

All the icons and their import paths are published in the [Bitrix24 icon catalog](https://bitrix24.github.io/b24icons/icons/).

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./components.md)
- [{#T}](./design.md)
- [{#T}](./app-logic.md)
- [{#T}](../placements.md)
