# Managing SIP Connections: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

SIP connections link Bitrix24 to a cloud or office PBX. The `voximplant.sip.*` methods allow you to:

- check the status of the SIP connector
- create and configure a SIP connection
- retrieve a list of application connections and monitor registration status
- update or delete a connection

To call the methods, you need the `Manage numbers - modify` access permission.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to connect a PBX via REST application](https://helpdesk.bitrix24.com/open/19882094/)

## Interaction with Other Objects

**Outgoing SIP Line.** After creating a SIP connection, its `CONFIG_ID` can be used in [voximplant.line.outgoing.sip.set](../lines/voximplant-line-outgoing-sip-set.md) to set the default outgoing SIP line.

**SIP Registration.** For cloud PBX, the [voximplant.sip.add](./voximplant-sip-add.md) and [voximplant.sip.get](./voximplant-sip-get.md) methods return the registration identifier `REG_ID`, for example, `150907`. Using it, the [voximplant.sip.status](./voximplant-sip-status.md) method returns the registration status in the `STATUS_RESULT` field:

- `success` — registration completed
- `error` — an error occurred during registration
- `in_progress` — registration is in progress
- `wait` — registration is waiting to start

An office PBX has no `REG_ID`: instead of registration, it receives the data for incoming calls in the `INCOMING_SERVER`, `INCOMING_LOGIN`, and `INCOMING_PASSWORD` fields.

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
5. If necessary, update the connection via [voximplant.sip.update](./voximplant-sip-update.md) or delete it via [voximplant.sip.delete](./voximplant-sip-delete.md). Deletion is irreversible, and if the line was the default outgoing line, another line becomes the outgoing one

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
