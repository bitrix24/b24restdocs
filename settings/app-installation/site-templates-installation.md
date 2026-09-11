# Installing Website Templates

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A website template is a configuration solution: a single-page or multi-page website packed into one archive. The solution works without the REST API — Bitrix24 itself creates the website pages from the archive.

The developer builds the website on their own Bitrix24, exports it to an archive, uploads the archive to the application card, and publishes the solution in the Bitrix24 Marketplace. The platform then installs the solution on the customer's Bitrix24.

Building the website itself — pages, blocks, and content — is described in the user documentation [Create and configure your Bitrix24 site](https://helpdesk.bitrix24.com/open/25743741/).

## When a Website Template Is the Right Choice

A website template is the right choice when the solution consists of ready-made pages and does not require its own code: for example, a landing page for an auto repair shop or a multi-page website for a clinic.

If the solution needs its own code and REST API calls, choose a mass-market application. If the solution is a set of CRM settings with pipelines, stages, and fields, choose an industry-specific CRM. If the automation is assembled from ready-made automation rules, choose smart scripts. All the options and their installation procedures are collected in the [{#T}](./index.md) article.

## Before You Start

- **Permissions.** To export a website, you need permission to view it. If the administrator has disabled website export, the export button is unavailable.
- **Developer's Area.** Publication options vary by region. Where free publication is available, accepting the partner catalog offer is sufficient. The regional terms are described in the [{#T}](../../market/preparing-to-publish/how-to-add-app.md) article.
- **Export result.** The website is exported to a separate archive. It is this archive that is uploaded to the application card.
- **Archive restrictions.** The requirements for the archive size, image size, and file name are collected in the [{#T}](../../market/preparing-to-publish/requirements-sites.md) article.

## How to Prepare a Website Template

1. Build the website on your own Bitrix24 and fill it with demo content. Pages are assembled from layout blocks — ready-made sections with text, images, and forms. Recommendations on structure and design are collected in the [{#T}](../../market/preparing-to-publish/requirements-sites.md) article.

2. Export the website to an archive: on the *Export settings* page of the Bitrix24 Marketplace, in the *Sites* group, select the *Export site* action. [Industry-specific CRMs](./vertical-crm-installation.md) and [smart scripts](./smart-scripts-installation.md) are exported in a similar way, but each solution type has its own action. The archive contains the website pages and the layout blocks they are built from.

3. Upload the archive to the application card in the Developer's Area — the procedure is described in the [{#T}](../../market/preparing-to-publish/how-to-add-app.md) article.

4. Publish the solution in the [Bitrix24 Marketplace](../../market/index.md). The publication requirements are collected in the [{#T}](../../market/preparing-to-publish/publication-requirements.md) article.

{% note warning "" %}

A website created in Bitrix24 with CoPilot cannot be exported to an archive. The restriction depends on how the website was created: if you built it yourself and CoPilot later refined the texts, images, or individual blocks, the export works

{% endnote %}

## How the Installation Works

The target Bitrix24 performs the installation; nothing is required from the developer at this step. Bitrix24:

1. downloads the archive from the Developer's Area
2. unpacks the archive
3. installs the applications from the retained list and adds their layout blocks to the repository of this Bitrix24
4. creates the website pages from the archive

How solutions of other types are installed is described in the [{#T}](./index.md) article.

## Marketplace Applications in a Website Template

While building the website before the export, the developer can install and use applications from the Bitrix24 Marketplace that add non-standard layout blocks. This way the template developer works on the structure and content of the website and uses ready-made design blocks provided by applications.

The list of the applications used is retained in the archive during the export. It includes the applications whose blocks are placed on the website pages and the applications that created the pages themselves.

{% note warning "" %}

If these applications include solutions available to the customer only under a Bitrix24 Marketplace subscription, your website template will also have to be published in the catalog under subscription terms

{% endnote %}

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./vertical-crm-installation.md)
- [{#T}](./smart-scripts-installation.md)
- [{#T}](../../market/preparing-to-publish/requirements-sites.md)