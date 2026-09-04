# Integration of UI Kit with REST API and Business Logic

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The UI Kit is responsible for the application interface. The data for the screen is retrieved and the permissions are checked separately, through the Bitrix24 REST API. Connecting the two is the job of the application code: obtain authorization inside the frame, call a method, and pass the response to a component.

## Where the Application Gets Authorization

An application built with the UI Kit opens inside Bitrix24, in a frame. There is no need to request a token or store it in the code: the [B24JsSDK](../../../sdk/b24jssdk/index.md) library receives the authorization data from the parent Bitrix24 window and refreshes the tokens on its own when they expire.

The connection is created by the `initializeB24Frame()` function. Until it completes, REST API calls are not available. If the page is opened directly in a browser instead of a frame, the function rejects the promise with an `SdkError` carrying the `JSSDK_CLIENT_SIDE_WARNING` code — this is why the call is wrapped in `try/catch`.

{% note warning "" %}

Do not authorize on the frontend through a webhook. The webhook URL contains a secret key, and any user will see it in the page code. A webhook is suitable only for the server side — for details, see the article [Installing and Using B24JsSDK](../../../sdk/b24jssdk/index.md).

{% endnote %}

## Example: A List of Users in a UI Kit Component

The component retrieves users with the [user.get](../../user/user-get.md) method and displays them in a UI Kit table. The application needs the `user` scope.

```html
<script setup>
import { onMounted, ref } from 'vue'
import { initializeB24Frame } from '@bitrix24/b24jssdk'

const users = ref([])
const error = ref('')
const isLoading = ref(true)

onMounted(async () => {
  try {
    const $b24 = await initializeB24Frame()

    const response = await $b24.actions.v2.call.make({
      method: 'user.get',
      params: { FILTER: { ACTIVE: true } },
      requestId: 'ui-kit-user-list'
    })

    if (!response.isSuccess) {
      error.value = response.getErrorMessages().join('; ')
      return
    }

    users.value = response.getData().result
  } catch (sdkError) {
    error.value = sdkError.message
  } finally {
    isLoading.value = false
  }
})
</script>

<template>
  <p v-if="isLoading">Loading</p>
  <p v-else-if="error">not_var{{ error }}</p>
  <B24Table v-else :data="users" />
</template>
```

What each part does:

- `initializeB24Frame()` returns the `$b24` object with authorization already in place
- `actions.v2.call.make()` calls a single REST API method. The `requestId` parameter is optional. It is sent along with the request and helps to find it in the logs
- `isSuccess` shows whether Bitrix24 returned a result, and `getErrorMessages()` returns the REST API error messages
- `getData().result` holds the response data. For the `user.get` method it is an array of employees
- `B24Table` accepts the array in the `data` property. The remaining properties of the component are described in the [Library Documentation](https://bitrix24.github.io/b24ui/docs/components/table/)

Installing the SDK, choosing a connection class, and working on the server are described in the article [Installing and Using B24JsSDK](../../../sdk/b24jssdk/index.md).

## The Approach Before the UI Kit

If you have already written applications for Bitrix24, you are familiar with this approach:

- the interface is created using PHP, HTML, and Bootstrap
- generated markup is displayed within a frame
- data is loaded with the `BX24.callMethod()` method from `BX24.js`
- REST API requests go directly from the browser or from PHP before the page is rendered

This approach worked and continues to work, but it has its limitations:

- the interface cannot be reused
- it is difficult to implement states: loading, errors, interactions
- CSS often had to be written manually for each screen
- a table has to be not only rendered but also updated manually

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

- buttons, forms, tables, layouts, modal windows
- components are responsive and match the Bitrix24 style
- you work not with HTML and styles, but with declarative components

The REST API itself remains the same: methods, parameters, and authorization work just like in BX24.js or PHP. What changes is the point of the call — from a Vue application, methods are called through B24JsSDK.

## What Remains the Same

- all REST API methods work exactly the same
- methods can be called from the application frontend or from your own server
- authorization has not changed: applications use the OAuth protocol, and the server side uses webhooks
- the application still runs inside a frame

## How the Roles Are Divided

#|
|| **Responsibility** | **Used to be: PHP and BX24.js** | **Now: Vue and UI Kit** ||
|| Layout | A PHP template | UI Kit components ||
|| Method call | `BX24.callMethod()` | `actions.v2.call.make()` from B24JsSDK ||
|| Result handling | Handled manually in JavaScript | Handled by Vue reactive variables ||
|| Interface elements | HTML and Bootstrap | UI Kit components ||
|#

## If You Have Not Worked With Vue

To get started, there is no need to know all of Vue:

- UI Kit components encapsulate behavior
- the project structure with Vite or Nuxt is simple
- it is enough to understand how to pass data and handle events

Start with the ready-made examples and figure things out as you go. Another option is to take only the design tokens and icons from the UI Kit and stay with HTML and Tailwind CSS. In that case, the components and their behavior remain unavailable.

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./quick-start.md)
- [{#T}](./components.md)
- [{#T}](../placement-bind.md)