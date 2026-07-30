# REST API Specifics in the Bitrix24 Self-Hosted Version

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A REST application works identically in both Bitrix24 Cloud and Bitrix24 Self-Hosted. However, the Self-Hosted version has specific characteristics that must be considered during implementation: the REST version may lag behind the Cloud version, the set of installed modules and updates depends on the owner, and part of the traffic goes through Bitrix24 Cloud and requires open network access.

Below are three recommendations to help your application work stably in the Self-Hosted version:

1. Check the availability of required methods and events during application installation
2. Refresh tokens only directly via the authorization server
3. Open network access to Bitrix24 servers in advance

## Checking Available Methods and Events

In Bitrix24 Cloud, the "latest version" of REST is always available. Updates for the Self-Hosted version are released some time after they appear in the Cloud. Bitrix24 Self-Hosted contains additional functionality, and verifying compatibility with it takes time.

Furthermore, for a specific Self-Hosted installation, there is no guarantee that the owner has installed the latest updates and all modules required by the application. For example, the Telephony module can be completely disabled in Bitrix24 Self-Hosted. Therefore, there is no certainty regarding the availability of specific REST methods during application installation.

**Recommendation number 1** — in your solution's installation script, verify the list of available methods and events using the [methods](../../../api-reference/common/system/methods.md) and [events](../../../api-reference/events/index.md) methods. If the required methods or events are missing, inform the user that they need to install updates on their Bitrix24.

## Bitrix24 Authorization Server

If your application refreshes authorization tokens using [`refresh_token`](../../oauth/auto-renewal.md), there are two options. The first is to obtain a new token pair by making a request to Bitrix24 itself:

```
xxx.bitrix24.xxx/oauth/token/?grant_type=refresh_token...
```

The second is to make a request directly to the `oauth.bitrix.info` authorization server:

```
oauth.bitrix.info/oauth/token/?grant_type=refresh_token...
```

In Bitrix24 Cloud, both options will work equally well. The Cloud simply "proxies" the request to the actual authorization server, whereas Bitrix24 Self-Hosted may not do this.

**Recommendation number 2** — always contact the `oauth.bitrix.info` authorization server directly to refresh tokens. This will work for both Cloud and Self-Hosted versions.

## Network Restrictions

When calling methods, the application makes a request directly to a specific Bitrix24 instance. Events reach the application differently. They pass through an event queue located in the cloud, even when dealing with a self-hosted Bitrix24 instance.

A situation may arise where the owner of a self-hosted Bitrix24 instance has closed network access to the event queue server. In this case, events simply will not work.

The same applies to the authorization server `oauth.bitrix.info`. If network access to it is closed, REST will not work in a self-hosted Bitrix24 instance. Bitrix24 will be unable to validate the token used by the application to access it.

**Recommendation number 3** — if you are implementing a solution with a client, ensure that the required Bitrix24 servers are whitelisted. Current server addresses and firewall configuration rules are collected in the article [Required Network Access](../network-access.md).

There is an alternative option — specifically for a particular client, you can connect a custom authorization and event mechanism on the side of their self-hosted Bitrix24 instance. For more details, see the article [Application Authorization in an Isolated Bitrix24 Self-Hosted Instance](custom-auth-provider.md) and the report by Maxim Sidorenko:

<iframe width="720" height="405" src="https://rutube.com/play/embed/0becef7c826427edd5e05e55ffb24144/?p=6tgNGVLNFCycVzj1phG1xg" frameBorder="0" allow="clipboard-write; autoplay" webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe>

## What's Next

- [Module Versioning in Self-Hosted Bitrix24](versions.md) — why a method or event might be unavailable and how to check the module version
- [Application Authorization in an Isolated Bitrix24 Self-Hosted Instance](custom-auth-provider.md) — using your own authorization and event mechanism for a client with a closed network
- [Adding Your Own Methods to the Bitrix24 Self-Hosted REST API](custom-methods.md) — how to extend REST with your own methods and scopes