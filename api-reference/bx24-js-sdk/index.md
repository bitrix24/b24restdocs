# Connecting and Using BX24.js

If your application implements a user interface within Bitrix24 (displayed on a special page or using embedding tools), you can take advantage of the capabilities of a special JS library. It:
- first, serves as a JS SDK for REST, allowing you to access REST directly from the front-end of your application without delving into [authorization implementation](../oauth/index.md) via OAuth 2.0,
- second, offers a range of additional features for interacting your application's user interface with the Bitrix24 interface:
  - frame size management,
  - calling standard dialogs,
  - etc.

The library is connected as follows:

```js
<script src="//api.bitrix24.com/api/v1/"></script>
```

The library cannot be used for external applications and webhooks.