# Managing SIP Connections: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section describes working with SIP connections in telephony:

- how to check the status of the SIP connector
- how to create and configure a SIP connection
- how to retrieve a list of application connections and monitor registration status
- how to update or delete a connection

To call the methods, you need the `Manage numbers - modify` access permission.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to connect a PBX via REST application](https://helpdesk.bitrix24.com/open/19882094/)

## Interaction with Other Objects

**Outgoing SIP Line.** After creating a SIP connection, its `CONFIG_ID` can be used in [voximplant.line.outgoing.sip.set](../lines/voximplant-line-outgoing-sip-set.md) to set the default outgoing SIP line.

**SIP Registration.** For cloud PBX, the response from [voximplant.sip.get](./voximplant-sip-get.md) returns a `REG_ID`. This identifier is used in [voximplant.sip.status](./voximplant-sip-status.md) to check the registration status.

## How to Choose the Connection Type

#| 
|| **Type** | **Description** | **When to Use** ||
|| `cloud` | Operator's cloud PBX | When connecting to an external SIP provider ||
|| `office` | Office PBX | When connecting to a local or office PBX ||
|#

{% note tip "User Documentation" %}

- [How to configure access permissions in telephony](https://helpdesk.bitrix24.com/open/18216960/)

{% endnote %}

## How to Get Started

1. Check the status of the SIP connector via [voximplant.sip.connector.status](./voximplant-sip-connector-status.md)
2. Create a connection using the [voximplant.sip.add](./voximplant-sip-add.md) method, specifying the type as `cloud` or `office`
3. Retrieve the list of connections via [voximplant.sip.get](./voximplant-sip-get.md) and select the desired `CONFIG_ID`
4. For cloud PBX, check the registration status using the [voximplant.sip.status](./voximplant-sip-status.md) method with `REG_ID`
5. If necessary, update the connection via [voximplant.sip.update](./voximplant-sip-update.md) or delete it via [voximplant.sip.delete](./voximplant-sip-delete.md)

## Overview of Methods {#all-methods}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can perform the method: user with the Manage numbers - modify permission

#| 
|| **Method** | **Description** ||
|| [voximplant.sip.add](./voximplant-sip-add.md) | Creates a SIP connection linked to the application ||
|| [voximplant.sip.update](./voximplant-sip-update.md) | Updates an existing SIP connection ||
|| [voximplant.sip.get](./voximplant-sip-get.md) | Returns a list of SIP connections created by the application ||
|| [voximplant.sip.status](./voximplant-sip-status.md) | Returns the status of SIP registration for cloud PBX ||
|| [voximplant.sip.delete](./voximplant-sip-delete.md) | Deletes an existing SIP connection ||
|| [voximplant.sip.connector.status](./voximplant-sip-connector-status.md) | Returns the current status of the SIP connector ||
|#