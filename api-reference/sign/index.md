# Signature: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The service for signing documents in Bitrix24 e-Signature for HR allows signing personnel documents with employees using a digital signature. It is equivalent to a handwritten signature and complies with legal requirements.

> Quick navigation: [all methods](#all-methods) 
>
> User documentation: [What is Bitrix24 Signature](https://helpdesk.bitrix24.com/open/20687752/), [How to set access permissions for the Signature section](https://helpdesk.bitrix24.com/open/21579950/)

The methods work with documents in the e-Signature for HR section — Signing with employees.

The methods are executed only in the context of authorization of the [application](../../settings/app-installation/index.md).

## Overview of Methods {#all-methods} 

> Scope: [`sign.b2e`](../scopes/permissions.md)
>
> Who can execute the method: a user with access to the company safe in the "signing with clients" section

#|
|| **Method** | **Description** ||
|| [sign.b2e.personal.tail](./sign-b2e-personal-tail.md) | Returns a list of documents signed by the user ||
|| [sign.b2e.mysafe.tail](./sign-b2e-mysafe-tail.md) | Returns a list of signed documents in the company safe ||
|#