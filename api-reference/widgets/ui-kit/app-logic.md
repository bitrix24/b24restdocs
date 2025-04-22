# Integration of UI Kit with REST API and Business Logic

Transitioning to the new UI Kit may raise questions for developers, especially if you have previously created applications for Bitrix24 using PHP or utilized `BX24.js` in static HTML pages. In this section, we will explain how the approach to building interfaces has changed, what remains the same, and how the connection between the visual part of the UI Kit and the Bitrix24 REST API now works.

## How It Used to Be

If you have already written applications for Bitrix24, you are familiar with the scheme:

- the interface is created using PHP, HTML, and Bootstrap,
- a generated markup is displayed within an iframe,
- data is loaded using `BX24.callMethod()` from `BX24.js`,
- REST requests were made directly from the browser or through PHP "before" the page was rendered.

This approach worked and continues to work, but it has its limitations:

- the interface cannot be reused,
- it is difficult to implement states: loading, errors, interactions,
- CSS often had to be written manually for each screen,
- everything is "in your hands": you draw a table — you also update it.

## What Has Changed

[Vue](https://vuejs.org/) has introduced a **reactive approach**: now you do not manage the DOM directly. Instead, you describe *what should be visible on the screen*, and Vue takes care of monitoring the state of the data and updating the interface when it changes.

A simple example:

```html
<script setup>
import { ref } from 'vue';

const count = ref(0);
</script>

<template>
  <button @click="count++">
    Clicked not_var{{ count }} times
  </button>
</template>
```

The value of `count` is a reactive variable. With each click, the value updates, and the interface automatically re-renders. You do not need to manually change the HTML — Vue does it for you.

With the UI Kit, you get ready-made Vue components that implement all the visual logic for you:

- buttons, forms, tables, layouts, modal windows,
- components are responsive and match the Bitrix24 style,
- you work not with HTML and styles, but with declarative components,
- the REST API is now called within the Vue application using fetch() or axios.

However, the REST API remains the same. Methods, parameters, authorization — everything works just like in BX24.js or PHP.

## What Remains the Same

- All REST API methods work exactly the same.
- You can call the API from the frontend or from your backend server.
- Authorization: OAuth 2.0, webhook — everything remains.
- The application still runs inside an iframe.

## Where Is the Logic Now?

Here’s how the roles are now divided:

| **Responsibility**       | **Used to be PHP + BX24.js**         | **Now Vue + UI Kit**            |
|---------------------------|----------------------------------|-------------------------------------|
| Layout                    | PHP template                      | UI Kit Vue components              |
| REST request              | BX24.callMethod()               | axios, fetch, useAsyncData         |
| Result handling           | JS manually                      | Vue reactive variables             |
| Table and other interface elements | HTML + Bootstrap                | Bitrix24 UI Kit components           |

## What If I Don’t Know Vue?

That’s okay. You don’t need to know all of Vue to start working:

- UI Kit components encapsulate behavior,
- project structure with Vite or Nuxt is simple,
- it’s enough to understand how to pass data and handle events.

You can start by copying examples and gradually figure it out. Additionally, you can use only design tokens and icons if you prefer to stick with HTML + Tailwind CSS for now. However, you won’t gain all the benefits of the UI Kit.