# Application Updates

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

This page describes what happens on Bitrix24 when an installed application is updated to a new version: how an update differs from the first installation, which data is retained, and what to consider in the new version's installer. After reading it, you will be able to plan the release of a new mass-market application version so that users' configured scenarios continue working.

This page does not cover the first installation of an application — it is described in [{#T}](app-installation/index.md). What happens during removal is described in the [{#T}](app-uninstallation.md) article.

## How a New Version Is Released

A new version of a mass-market application follows the same path as the first one. The developer adds the version on the *Version History* tab in the solution card and submits it for moderation. After the first version passes moderation, the solution is published to the catalog automatically; for subsequent versions, the developer controls the *Available in the catalog* checkbox. For details, see the [{#T}](../market/preparing-to-publish/publication-requirements.md) article.

Each Bitrix24 instance updates to the published version separately. The application is not reinstalled: `client_id` and `client_secret` remain unchanged, and the installation history is retained.

## What Happens During an Update {#effects}

There is no separate application update procedure: Bitrix24 performs the same installation as the first time, except that it updates the existing application record instead of creating one. Bitrix24 also re-evaluates how the installation is completed, and this determines both the wizard behavior and what happens to the application's service subscriptions.

#|
|| **Application Type** | **Installation Wizard** | **Service Subscriptions** ||
|| Application with an [installation wizard](app-installation/mass-market-apps/installation-master.md) — the initial installation path is specified | The installation completion flag is reset. The installation page opens for the user again, and the new version must call [installFinish](app-installation/installation-finish.md) again. Until this call is received, the application is considered unconfigured, and its functionality is blocked | The `ONAPPINSTALL` subscription is removed and is not created again — even if the application created it itself with [event.bind](../api-reference/events/event-bind.md). The `ONAPPUSERREADY` subscription is recreated for the same installation path ||
|| Application without an interface, API-only, or without an initial installation path | Does not open, and the application remains installed | The `ONAPPINSTALL` and `ONAPPUSERREADY` subscriptions are recreated for the installation path, and the application receives the [ONAPPINSTALL](../api-reference/common/events/on-app-install.md) event on every update ||
|#

This leads to two practical consequences. The installer must work correctly not only on a fresh Bitrix24 instance, but also where the previous version has already configured everything. An application with an installation wizard cannot rely on its own `ONAPPINSTALL` subscription: the subscription will not exist after an update, so the application will learn about the update only when the user opens the installation page.

## What Is Retained During an Update {#persists}

An update does not affect the data the application stores on Bitrix24: CRM records, files, list items. Registrations made through REST are also retained, except for the two service subscriptions listed in the table above.

#|
|| **What** | **What Happens During an Update** ||
|| `client_id` and `client_secret` | Remain unchanged. The application remains the same, and no repeated OAuth registration is required ||
|| [Event handlers](../api-reference/events/index.md), except `ONAPPINSTALL` and `ONAPPUSERREADY` | Retained. Only removing the application cancels these subscriptions ||
|| [Widget handlers](../api-reference/widgets/index.md) | Retained under the same condition ||
|| [Application data stores](../api-reference/entity/index.md) | Retained. They are cleared only when the application is removed with the *Delete application settings and data* option enabled ||
|| [Application settings](../api-reference/common/settings/index.md) | Retained under the same condition ||
|#

## New Version Release Checklist {#checklist}

1. **Make the installer idempotent.** The same code runs during both the first installation and an update. Re-registering an existing object must not break the installation.
2. **Do not create duplicates when registering again.** Most registrations survive an update, so before [event.bind](../api-reference/events/event-bind.md), retrieve current subscriptions with [event.get](../api-reference/events/event-get.md); before [placement.bind](../api-reference/widgets/placement-bind.md), use [placement.get](../api-reference/widgets/placement-get.md), and register only what is missing. The exceptions are `ONAPPINSTALL` and `ONAPPUSERREADY`: Bitrix24 recreates these subscriptions itself.
3. **Do not change the codes of automation rules and workflow actions between versions.** Automation scenarios and workflows refer to an automation rule by its code. A new version with a different code will leave the configured scenarios broken.
4. **Store the version of your data structure separately.** Record it in the [application data store](../api-reference/entity/index.md) or on your own side so that the new version's installer knows which migrations to apply.
5. **Test the update on a test Bitrix24 instance.** Install the previous version, configure scenarios on it — automation rules in pipelines, widgets, event subscriptions — and update to the new version. Make sure the installation wizard runs again and calls `installFinish`. Testing only a fresh installation is not enough.
6. **Ensure backward compatibility of handlers.** Users on different Bitrix24 instances do not update simultaneously, and your server will receive requests from both the old and new versions for some time.

## Continue Learning

- [{#T}](app-installation/index.md)
- [{#T}](app-uninstallation.md)
- [{#T}](app-installation/installation-finish.md)
- [{#T}](app-installation/mass-market-apps/installation-master.md)
