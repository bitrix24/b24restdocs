# Files: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods allow you to upload files to the chat on behalf of the bot and obtain a download link for the already uploaded file.

> Quick navigation: [all methods](#all-methods)

## How to Work with Files

- use [imbot.v2.File.upload](./file-upload.md) to upload a file to the chat and, if necessary, send it along with a message.
- use [imbot.v2.File.download](./file-download.md) to get a link for downloading the file.

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

#| 
|| **Method** | **Description** ||
|| [imbot.v2.File.upload](./file-upload.md) | Uploads a file to the chat ||
|| [imbot.v2.File.download](./file-download.md) | Returns a link for downloading the file ||
|#

## Continue Your Exploration

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](../../index.md)
- [Messages imbot.v2](../messages/index.md)