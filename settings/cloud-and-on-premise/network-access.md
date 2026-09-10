# Required Network Access

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The on-premise Bitrix24 is deployed inside the company network, while the application runs on its own server, so requests between them pass through the corporate firewall. If a required direction is closed, the application is not installed and events do not reach the handler.

Cloud Bitrix24 requires no access configuration: the cloud is reachable from the internet. Apply the rules below when the on-premise Bitrix24, the application server, or both sides at once are located inside a closed perimeter.

Configuring the firewall itself, NAT, and the proxy remains the responsibility of the network administrator.

## How Requests Travel

#|
|| **Participant** | **Role in the Exchange** ||
|| On-premise Bitrix24 | Calls external Bitrix24 services, accepts REST method calls from the application, and sends events to the authorization server ||
|| Authorization server `oauth.bitrix.info` | Issues and refreshes application tokens, receives events from the on-premise Bitrix24, and passes them to the queue. Details are in the article [Complete OAuth 2.0 Authorization Protocol](../oauth/index.md) ||
|| Queue servers `mp_actions-*` | Send event, automation rule, and workflow action calls to the application server ||
|| Application server | Hosts the application code and the handlers, accepts calls from the queue servers ||
|#

An event travels from the on-premise Bitrix24 to the authorization server, and then a queue server sends a POST request to the handler. That is why the on-premise Bitrix24 does not need separate access to the queue servers: outgoing access to the authorization server is enough. On the application side, incoming requests from the queue servers are required. The mechanism is described in the [Events](../../api-reference/events/index.md) section.

Handlers of [outbound webhooks](../../local-integrations/local-webhooks.md) are called from the same queue servers — there is no need to open separate directions for them.

The scheme changes if the on-premise Bitrix24 uses a [custom authorization provider](./on-premise/custom-auth-provider.md). In this case the Bitrix24 authorization server drops out of the scheme, and events are sent as a direct POST request from the on-premise Bitrix24 itself. Access to the authorization server and to the queue servers is not required, and the application server opens incoming requests from the on-premise Bitrix24.

Directions in the tables are given from the point of view of the server being configured: outgoing are the requests it sends itself, incoming are the requests it accepts. In rows with outgoing requests, the address is the destination; in rows with incoming requests, it is the source. The https protocol uses port 443, http uses port 80.

Access from employee workstations to the application server is not covered here — the rules below describe only the exchange between servers.

## Access on the On-Premise Bitrix24 Side

#|
|| **Address** | **Direction** | **Why It Is Needed** | **If Access Is Closed** ||
|| `oauth.bitrix.info` | Outgoing https | Application mechanism: installation, issuing and refreshing tokens, sending events to the queue | The application is not installed, tokens are not issued or refreshed, events do not reach the handlers ||
|| `*.bitrixsoft.com` | Outgoing http and https | Developer resources section, where integrations and webhooks are created | The Developer resources section does not work, an integration or a webhook cannot be created ||
|| `https://util.bitrixsoft.com/` | Outgoing https | Operation of the application storefront — the list of Bitrix24 Market applications in the on-premise Bitrix24. The address falls under the `*.bitrixsoft.com` mask, so a separate entry is not required when access is granted by mask | The application storefront does not work ||
|| `https://www.bitrix24.*/util/` | Outgoing https | Event names in the outbound webhook creation interface. Choose the domain that matches the region of your Bitrix24 | Event names are not displayed in the outbound webhook creation interface ||
|| Application servers | Incoming https | REST method calls from the application | The application cannot call REST methods ||
|#

The server address depends on the specific application. For your own application, this is the address of your server. For a third-party application, this is the address from its description in Bitrix24 Market; if it is not there, request the address from the developer.

## Access on the Application Server Side

#|
|| **Address** | **Direction** | **Why It Is Needed** | **If Access Is Closed** ||
|| `oauth.bitrix.info` | Outgoing https | Exchanging the authorization code for tokens and refreshing tokens | The application will not receive or refresh tokens ||
|| On-premise Bitrix24 address | Outgoing https | REST method calls | The application cannot work with Bitrix24 data ||
|| `https://dl.bitrix24.com/webhook/app-world.json` | Outgoing https | [List of IP addresses](#nodes) of the queue servers for the firewall rules | The list of addresses is not updated, and some calls stop getting through ||
|| Server group `mp_actions-*`, [list of IP addresses](#nodes) | Incoming https | Events, automation rules, and workflow actions | The handler does not receive events, automation rule calls, or workflow action calls ||
|#

The handler must accept incoming requests with the `application/x-www-form-urlencoded` data type — call data arrives in this format. Make sure that the firewall or the proxy does not drop such requests.

Open incoming access does not confirm that a request came from Bitrix24, so check the `application_token` parameter in the handler. Accept calls over https: over http, the token and the event data travel in clear text. The verification procedure is described in the article [Security Recommendations for Applications Using REST API](./security-recommendations.md).

## How to Retrieve the List of Queue Server IP Addresses {#nodes}

Incoming requests come from a dynamic group of servers, also called scale-based. The size of the group changes under load, so the set of IP addresses is not constant. `mp_actions-*` is a template of the group name: it contains no specific addresses, so firewall rules are built from the list of IP addresses.

The current list is returned by the request:

```bash
curl https://dl.bitrix24.com/webhook/app-world.json
```

The response — the number of addresses in it varies:

```json
{
    "nodes": ["3.217.33.54", "52.29.163.104"]
}
```

Use the addresses from the `nodes` array in the firewall rules for incoming connections in the corporate network or on the on-premise virtual machine.

A new server appears in the `nodes` list 5-10 minutes before calls start arriving from it. Poll the address once every 5-10 minutes. The request must not be sent more often than once per minute. Schedule the update of the rules based on the `nodes` list.

Previously, specific IP addresses that had to be opened for requests were published. Now a fixed list of IP addresses is used only for the queue servers and is taken from `nodes`. Rules for the other directions are defined by domain names.

## How to Check Access

Check outgoing access from the server where the rules were configured. The request must return a server response, not a connection error or a timeout.

```bash
curl -I https://oauth.bitrix.info/
```

A successful response starts with the status line:

```text
HTTP/2 200
```

The messages `Connection timed out` and `Connection refused` mean that access is closed.

Check incoming access to the handler from an external host that is not part of the company network. The `-d` parameter sends the request with the `application/x-www-form-urlencoded` data type — calls from the queue servers arrive in the same format. The handler must respond rather than drop the connection.

```bash
curl -d "test=1" https://example.com/handler.php
```

## Continue Learning

- [{#T}](./on-premise/index.md)
- [{#T}](./security-recommendations.md)
- [{#T}](../oauth/index.md)
- [{#T}](../../api-reference/events/index.md)
- [{#T}](../../local-integrations/local-webhooks.md)
