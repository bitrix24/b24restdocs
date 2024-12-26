{% include notitle [Note on required parameters](required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **access_token**
[`string`](../api-reference/data-types.md) | Authorization token [OAuth 2.0](../api-reference/oauth/index.md) ||
|| **expires_in**
[`integer`](../api-reference/data-types.md) | Time in seconds until the token expires ||
|| **scope***
[`string`](../api-reference/data-types.md) | [Scope](../api-reference/scopes/permissions.md) under which the event occurred ||
|| **domain***
[`string`](../api-reference/data-types.md) | Bitrix24 address where the event occurred ||
|| **server_endpoint***
[`string`](../api-reference/data-types.md) | Bitrix24 authorization server address needed for refreshing OAuth 2.0 tokens ||
|| **status***
[`string`](../api-reference/data-types.md) | Status of the application that subscribed to this event:

- `L` — [local](../local-integrations/local-apps.md) application
- `F` — [free mass-market](../market/index.md) application
- `S` — [subscription-based mass-market](../market/monetization/index.md) application

||
|| **client_endpoint***
[`string`](../api-reference/data-types.md) | Common path for REST API method calls for Bitrix24 where the event occurred ||
|| **member_id***
[`string`](../api-reference/data-types.md) | Bitrix24 identifier where the event occurred ||
|| **refresh_token**
[`string`](../api-reference/data-types.md) | Token for extending authorization [OAuth 2.0](../api-reference/oauth/index.md) ||
|| **application_token***
[`string`](../api-reference/data-types.md) | Token for secure event handling ||
|#

{% note alert "" %}

Authorization tokens are not always passed to the event handler. If the hit that initiated the event could not be linked to a specific Bitrix24 user, the tokens are not passed. Always check the contents of the auth key in your code.

It is recommended to store tokens obtained earlier during the application installation. Use them when working with the application interface in the form of embeds, widgets, and so on.

{% endnote %}