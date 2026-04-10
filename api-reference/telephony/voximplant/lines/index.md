# Managing Lines: Overview of Methods

This section describes how to work with outgoing lines in telephony:

- how to obtain a list of available lines
- how to find out the current default outgoing line
- how to set a regular or SIP line for outgoing calls

To call the methods, you need the `Manage numbers - modify` access permission.

> Quick navigation: [all methods](#all-methods)

## Interaction with Other Objects

**Outgoing Line.** The methods use the line identifier `LINE_ID`. You can obtain it through [voximplant.line.get](./voximplant-line-get.md) and then pass it to [voximplant.line.outgoing.set](./voximplant-line-outgoing-set.md) to change the default line.

**SIP Line.** To set the default SIP line, use `CONFIG_ID` in [voximplant.line.outgoing.sip.set](./voximplant-line-outgoing-sip-set.md). The `CONFIG_ID` identifier can be obtained using the [voximplant.sip.get](../sip/voximplant-sip-get.md) method.

{% note tip "User Documentation" %}

- [How to Set Up Access Permissions in Telephony](https://helpdesk.bitrix24.com/open/18216960/)

{% endnote %}

## Getting Started

1. Obtain a list of available outgoing lines via [voximplant.line.get](./voximplant-line-get.md)
2. Check the current outgoing line using the [voximplant.line.outgoing.get](./voximplant-line-outgoing-get.md) method
3. Set the desired line through [voximplant.line.outgoing.set](./voximplant-line-outgoing-set.md) or the SIP line through [voximplant.line.outgoing.sip.set](./voximplant-line-outgoing-sip-set.md)
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