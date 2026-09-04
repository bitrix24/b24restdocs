# Installing Smart Scripts

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Smart scripts are a configuration solution: a set of CRM automation rules that are run for selected leads, deals, companies, and contacts outside CRM pipelines. The solution works without the REST API — Bitrix24 itself applies the scenarios from the archive.

The developer builds the scripts on their own Bitrix24, exports them to an archive, uploads the archive to the application card, and publishes the solution in Bitrix24 Market. After that, the platform itself installs the solution on the client's Bitrix24.

## When Smart Scripts Are a Good Fit

Smart scripts are a good fit when the automation is assembled from ready-made automation rules and requires no code of your own: for example, a mailing to selected contacts or a sequence of actions for selected companies.

If the solution needs its own code and calls to the REST API, choose a mass-market application. If the solution is a complete CRM configuration, with pipelines, stages, and fields, choose an industry-specific CRM. If it is a ready-made website, choose site templates. All the options and the order of their installation are collected in the article [{#T}](./index.md).

## What You Need Before You Start

- **Permissions.** Only a Bitrix24 administrator can export and import smart scripts.
- **Developer's Area.** For free publication, it is enough to accept the offer of the partner catalog. The terms are described in the article [{#T}](../../market/preparing-to-publish/how-to-add-app.md).
- **Export result.** The scenarios are exported to a separate archive. It is this archive that is uploaded to the application card.

## How to Prepare the Solution

1. Build the scripts on your own Bitrix24, in the CRM section. The configuration order and the requirements for the description are collected in the article [{#T}](../../market/preparing-to-publish/requirements-smart-scripts.md).

2. Export the scripts to an archive. This export works separately from the CRM settings export used for [Industry-Specific CRMs](./vertical-crm-installation.md).

3. Upload the archive to the application card in the Developer's Area — the procedure is described in the article [{#T}](../../market/preparing-to-publish/how-to-add-app.md).

4. Publish the solution in [Bitrix24 Market](../../market/index.md). The publication requirements are collected in the article [{#T}](../../market/preparing-to-publish/publication-requirements.md).

## How Installation Works

The installation is performed by the target Bitrix24 account, and nothing is required from the developer at this step. Bitrix24:

1. downloads the archive from the Developer's Area
2. unpacks the archive
3. adds the scripts from the archive files to the CRM objects — leads, deals, companies, and contacts

How solutions of other types are installed is described in the article [{#T}](./index.md).

## Market Applications in Smart Scripts

When configuring the scripts before the export, the developer can install and use applications from Bitrix24 Market. For example, a non-standard automation rule from a ready-made solution can be used in a script. This way the developer of smart scripts works on the business logic and reuses ready-made automation rules instead of developing each one — how application automation rules are built is described in the overview [{#T}](../../api-reference/bizproc/bizproc-robot/index.md).

The list of the applications used is retained in the archive during the export. Upon import, the applications from this list are installed automatically on the same Bitrix24 where the smart scripts are installed.

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./vertical-crm-installation.md)
- [{#T}](./site-templates-installation.md)
- [{#T}](../../market/preparing-to-publish/requirements-smart-scripts.md)
- [{#T}](../../api-reference/bizproc/bizproc-robot/index.md)
