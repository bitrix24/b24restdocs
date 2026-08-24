{% note warning "Code on the Bitrix24 domain only" %}

The `Messenger` object is available only to page code and extensions — the code that runs on the Bitrix24 page itself, on the same origin. The object is reached through `import { Messenger } from 'im.public'` or `BX.Runtime.loadExtension('im.public.iframe')`, both of which give direct access to the messenger runtime.

An application runs inside its own iframe on a different origin and has no access to that runtime: neither `Messenger.*` nor `BX.Runtime.loadExtension` is reachable from it. To open a chat from an application, use [BX24.im.openMessenger](/sdk/bx24-js-sdk/additional-functions/bx24-im-open-messenger.html) or `$b24.parent.imOpenMessenger` in the [b24jssdk](https://bitrix24.github.io/b24jssdk/docs/working-with-the-rest-api/frame-parent) library.

{% endnote %}
