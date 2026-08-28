# Installation of Industry-Specific CRMs

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An industry-specific CRM is a configuration solution: a set of CRM settings saved into a single archive. The archive holds deal pipelines, lead stages, custom fields of CRM objects, and the settings of Automation rules and workflows. The solution works without the REST API — Bitrix24 applies the settings from the archive on its own.

The developer configures the CRM on their own Bitrix24, exports the settings to a file, uploads the archive to the application card, and publishes the solution in the Bitrix24 Marketplace. From there, the platform installs the solution on the customer’s Bitrix24 itself.

This page covers preparing and publishing the solution. Configuring the CRM itself — creating pipelines, fields, and Automation rules — is described in the Bitrix24 user documentation.

## When an Industry-Specific CRM Fits

An industry-specific CRM fits when the solution consists of settings alone and requires no code of its own: for example, a ready-made workflow for a car service, a clinic, or a real estate agency.

If the solution needs its own code and REST API calls, choose a mass-market application. If it needs Automation rules outside the pipelines, choose smart scripts; if it is a ready-made website, choose site templates. All the options and their installation order are collected in the [{#T}](./index.md) article.

## What You Need Before You Start

- **Permissions.** Only a Bitrix24 administrator can export and upload CRM settings.
- **Developer’s area.** The application card is created in the Developer’s area — the procedure is described in the [{#T}](../../market/preparing-to-publish/how-to-add-app.md) article.
- **Export result.** The settings are exported into a zip archive. That archive is what you upload to the application card.

## How to Prepare an Industry-Specific CRM

1. Configure the CRM on your own Bitrix24: pipelines and stages, custom fields of objects, Automation rules, and workflows.

2. Export the settings into a zip archive. In the CRM Kanban, find the *Industry-specific CRMs for your business* block and select *Export your CRM to a file*. The *Configure* button in the same block opens the industry solutions panel.

3. Upload the archive to the application card in the Developer’s area — the procedure is described in the [{#T}](../../market/preparing-to-publish/how-to-add-app.md) article.

4. Publish the solution in the [Bitrix24 Marketplace](../../market/index.md). The publication requirements are collected in the [{#T}](../../market/preparing-to-publish/publication-requirements.md) article.

## How the Installation Runs

The target Bitrix24 installs the solution automatically, and nothing is required from the developer at this step. It:

1. downloads the archive from the Developer’s area
2. unpacks the archive
3. applies the settings from the archive files

How solutions of other types are installed is described in the [{#T}](./index.md) article.

## Marketplace Applications in an Industry-Specific CRM

While configuring the CRM before the export, the developer can install and use applications from the Bitrix24 Marketplace. For example, they can reuse non-standard Automation rules from a ready-made solution, or connect an application that formats customer phone numbers. This way the developer of an industry-specific CRM focuses on the business logic and reuses existing Automation rules and integrations.

The list of the applications used is retained in the industry-specific CRM file during the export. On import, the applications from this list are installed automatically on the target Bitrix24.

{% note warning "" %}

If these applications include solutions available to the customer only under a Bitrix24 Marketplace subscription, your industry-specific CRM will also have to be published in the catalog under subscription terms

{% endnote %}

## Continue Your Exploration

- [{#T}](./smart-scripts-installation.md)
- [{#T}](./site-templates-installation.md)
- [{#T}](./index.md)