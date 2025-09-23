# Installation Options for Applications in Bitrix24

When planning user scenarios to be implemented in the application, it's important to understand how the installation process works for a specific Bitrix24 account.

First of all, it's necessary to distinguish between the installation of [local applications](./local-apps/index.md) and [mass-market solutions](./mass-market-apps/index.md). Therefore, if you initially plan to develop a mass-market solution, it's advisable to start development with the application added to the Developer's area instead of a "test local" application. Otherwise, after debugging the installation process for a local application, you may find that the installation script needs to be changed for the mass-market solution.

A separate installation scenario is implemented for so-called "configuration" solutions: [industry CRMs](./vertical-crm-installation.md), packages with "[smart scenarios](./smart-scripts-installation.md)", [website](./site-templates-installation.md) templates, BI constructor templates, and other types of solutions that are not REST applications but can be published in the Bitrix24 Marketplace.