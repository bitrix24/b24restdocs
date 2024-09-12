# Options for Installing Applications in Bitrix24

When planning user scenarios to be implemented in the application, it's important to understand how the installation process works on a specific Bitrix24 account.

First of all, it's necessary to distinguish between the installation of [local applications](./local-apps/index.md) and [mass-market solutions](./mass-market-apps/). Therefore, if you initially plan to develop a mass-market solution, it's preferable to start development by adding the application in the Developer's area instead of creating a "test local" application. Otherwise, after debugging the installation process for a local application, you may find that the installation scenario needs to be changed for the mass-market solution.

A separate installation scenario is implemented for so-called "configuration" solutions: [industry CRMs](./vertical-crm-installation.md), packages with "[smart scenarios](./smart-scripts-installation.md)", templates for [websites](./site-templates-installation.md), BI constructor templates, and other types of solutions that are not REST applications but can be published in the Bitrix24 Marketplace.