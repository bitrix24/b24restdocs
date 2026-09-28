# File Version: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A file version is a saved snapshot of a file at a specific point in time. When a file is modified, the system creates a new version. This allows tracking the history of changes and restoring files to the desired state.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [File version storage](https://helpdesk.bitrix24.com/open/18874394/)

## How to Start

The method [disk.version.get](./disk-version-get.md) returns information about a file version, including its size, creation time, the user ID of the person who created this version, and a temporary link for downloading the version.

To retrieve version data:

1. Request a list of file versions using the method [disk.file.getVersions](../file/disk-file-get-versions.md). In the response, you will receive an array with the `ID` of all available versions
2. Use the required `ID` as a parameter in the method [disk.version.get](./disk-version-get.md)

## What the Method Returns

The method `disk.version.get` returns file version data. Here is a shortened response example:

```json
{
    "result": {
        "ID": "7169",
        "NAME": "Picture.png",
        "SIZE": "52486",
        "CREATE_TIME": "2025-12-23T10:30:01+03:00",
        "DOWNLOAD_URL": "https://test.bitrix24.com/rest/download.json?..."
    }
}
```

`DOWNLOAD_URL` is a temporary link for downloading the version. If a version with the specified `ID` is not found, the method returns the `ERROR_NOT_FOUND` error. If the user does not have permission to read the file, it returns `ACCESS_DENIED`. The full response structure and error examples are provided in the description of the [disk.version.get](./disk-version-get.md) method.

## Overview of Methods {#all-methods}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required file

#|
|| **Method** | **Description** ||
|| [disk.version.get](./disk-version-get.md) | Returns the file version ||
|#
