# Managing Lines: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An outgoing line specifies the number or connection used for employees' calls. The `voximplant.line.*` methods let you:

- retrieve a list of available lines
- identify the current default outgoing line
- set a line for outgoing calls

To call the methods, you need the `Manage numbers - modify` access permission.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [General Telephony Settings](https://helpdesk.bitrix24.com/open/8748685/)

## Choosing a Method to Set the Line

To set the outgoing line, pass either the line ID `LINE_ID` or the SIP connection ID `CONFIG_ID`. Choose the method based on which ID you have.

#|
|| **If You Have** | **Method** | **Pass** ||
|| A line ID from [voximplant.line.get](./voximplant-line-get.md), including a SIP line | [voximplant.line.outgoing.set](./voximplant-line-outgoing-set.md) | The line ID `LINE_ID`, such as `reg150907` or `sip7` ||
|| A SIP connection ID for the current application from [voximplant.sip.get](../sip/voximplant-sip-get.md) | [voximplant.line.outgoing.sip.set](./voximplant-line-outgoing-sip-set.md) | The connection ID `CONFIG_ID`, such as `9` ||
|#

`LINE_ID` and `CONFIG_ID` are not interchangeable: `LINE_ID` identifies a line, while `CONFIG_ID` identifies a SIP connection configuration.

{% note tip "User Documentation" %}

- [How to Set Up Access Permissions in Telephony](https://helpdesk.bitrix24.com/open/18216960/)

{% endnote %}

## Getting Started

1. Check the current outgoing line using [voximplant.line.outgoing.get](./voximplant-line-outgoing-get.md)
2. Retrieve `LINE_ID` using [voximplant.line.get](./voximplant-line-get.md) or `CONFIG_ID` using [voximplant.sip.get](../sip/voximplant-sip-get.md)
3. Choose a method from the table above and pass the corresponding ID
4. Re-invoke [voximplant.line.outgoing.get](./voximplant-line-outgoing-get.md) to verify the currently set outgoing line

## Overview of Methods {#all-methods}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can perform the method: a user with the Manage numbers - modify access permission

#| 
|| **Method** | **Description** ||
|| [voximplant.line.get](./voximplant-line-get.md) | Returns a list of available outgoing lines ||
|| [voximplant.line.outgoing.get](./voximplant-line-outgoing-get.md) | Returns the identifier of the current default outgoing line ||
|| [voximplant.line.outgoing.set](./voximplant-line-outgoing-set.md) | Sets the default outgoing line ||
|| [voximplant.line.outgoing.sip.set](./voximplant-line-outgoing-sip-set.md) | Sets the default outgoing SIP line ||
|#
