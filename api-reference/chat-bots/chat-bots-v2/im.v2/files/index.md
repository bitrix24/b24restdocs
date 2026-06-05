# Files: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods allow you to upload files to the chat and receive a download link for the file on behalf of the user or application.

> Quick navigation: [all methods](#all-methods)

## How to Work with Files

- use [im.v2.File.upload](./file-upload.md) to upload a file to the chat
- use [im.v2.File.download](./file-download.md) to get a download link for an already uploaded file

## Overview of Methods {#all-methods}

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute the methods: a user or an application with access to the messenger

#| 
|| **Method** | **Description** ||
|| [im.v2.File.upload](./file-upload.md) | Uploads a file to the chat ||
|| [im.v2.File.download](./file-download.md) | Returns a link to download the file ||
|#

## Continue Your Exploration

- [API imbot.v2 Change Log](../../change-log.md)
- [im.v2: Overview of Methods](../index.md)
- [im.v2 Events](../events/index.md)