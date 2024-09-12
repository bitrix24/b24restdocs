{% include notitle [Note on required parameters](required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **access_token**
[`string`](../api-reference/data-types.md) | Authorization token [OAuth 2.0](../api-reference/oauth/) ||
|| **expires_in**
[`integer`](../api-reference/data-types.md) | Time in seconds until the token expires ||
|| **scope***
[`string`](../api-reference/data-types.md) | [Scope](../api-reference/scopes/permissions.md) under which the event occurred ||
|| **domain***
[`string`](../api-reference/data-types.md) | Address of Bitrix24 where the event occurred ||
|| **server_endpoint***
[`string`](../api-reference/data-types.md) | Address of the Bitrix24 authorization server needed to refresh OAuth 2.0 tokens ||
|| **status***
[`string`](../api-reference/data-types.md) | Status of the application that subscribed to this event:

- `L` — [local](../local-integrations/local-apps.md) application
- `F` — [free mass-market](../market/) application
- `S` — [subscription mass-market](../market/monetization/) application

||
|| **client_endpoint***
[`string`](../api-reference/data-types.md) | Common path for REST API method calls for Bitrix24 where the event occurred ||
|| **member_id***
[`string`](../api-reference/data-types.md) | Identifier of Bitrix24 where the event occurred ||
|| **refresh_token**
[`string`](../api-reference/data-types.md) | Authorization renewal token [OAuth 2.0](../api-reference/oauth/) ||
|| **application_token***
[`string`](../api-reference/data-types.md) | Token for secure event handling ||
|#

{% note alert "Please note!" %}

It is important to consider that authorization tokens are not always passed to the event handler.

If the hit that triggered the event could not be linked to a specific Bitrix24 user, the tokens are not passed to the handler. Be sure to check the contents of the `auth` key in your code!

We recommend storing and using tokens obtained earlier during the application installation when users interact with the application interface in the form of widget embeds, and so on.

{% endnote %}