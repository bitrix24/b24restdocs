{% note warning "The application runs in an iframe" %}

An application runs inside its own iframe on a different origin. The Bitrix24 runtime is not available there — neither `Messenger.*` nor `BX.Runtime.loadExtension` can be reached from the application.

The only channel outward is the parent bridge: the `BX24.im.*` methods, or their equivalents `$b24.parent.imCallTo`, `$b24.parent.imPhoneTo`, `$b24.parent.imOpenMessenger`, and `$b24.parent.imOpenHistory` in the [b24jssdk](https://bitrix24.github.io/b24jssdk/docs/working-with-the-rest-api/frame-parent) library.

Calling these methods prints a deprecation notice in the Bitrix24 console that recommends `Messenger.startVideoCall`, `Messenger.startPhoneCall`, or `Messenger.openChat`. Disregard it: those are top-window methods, available only to code running on the Bitrix24 page itself, and they are not reachable from an application.

The `im.public.iframe` extension that the notice names is meant for an iframe on the Bitrix24 domain, not for an application frame. For that case, see [Passing Context to the Bot When Opening a Chat](/api-reference/chat-bots/chat-bots-v2/imbot.v2/bot-context.html).

{% endnote %}
