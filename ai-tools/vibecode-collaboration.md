---
title: 'Collaborating on an App in Alaio Vibecode'
description: 'How several employees work on the code of a single app in Alaio Vibecode: the server development team, member roles, protection against overwriting the work of others on deployment, and handing an app over to another owner.'
---

Several employees can work on the app code, and the app itself can be handed over to another owner together with the keys, the server, and the sources. Everyone works with their own Vibecode key — there is no need to share someone else's key.

## Server Development Team {#team}

The server owner assembles a development team: adds Bitrix24 employees and assigns a role to each of them. The team roster is managed by the server owner or a Bitrix24 administrator, on the *Collaboration* tab of the server or app card.

A member works with their own personal key that has the `vibe:infra` permission. The server is not rebound to another key, and the owner's key stays with the owner only. Permissions come from team membership, not from the key: when an employee is removed from the team, access is closed immediately.

#|
|| **Role** | **What is available** ||
|| **Developer** | App deployment, commands on the server, files, logs, source storage, waking the server up ||
|| **Administrator** | Everything above plus server management: lifecycle, configurations and security, plan and disk, backups, catalog card, app audience, and spending ||
|#

In the API, the roles are named `DEVELOPER` and `ADMIN`. A member sees their own role and the list of available actions in the server card and in the server response.

Deleting the server, changing the owner, access links, and rebinding the key remain with the server owner. Deleting source versions, like managing the team roster, is available to the owner and to a Bitrix24 administrator.

Along with being added to the team, an employee gets the right to open the app itself. This right is not revoked automatically: when an employee leaves the team, it is removed separately, on the access tab.

## Protection Against Overwriting Someone Else's Work {#base-version}

On deployment, you specify which source version it is based on. If a newer version has appeared in the storage, the deployment is rejected with the `SOURCE_VERSION_STALE` code: the response contains the number of the current version and a link to download it.

On a server with a development team, specifying the version is mandatory as soon as the storage holds at least one. On a server without a team, this is left to the developer's discretion.

{% note tip "" %}

For how to pass the version on deployment and which rejection codes are returned, see the Vibecode documentation, [App deployment](https://vibecode.bitrix24.com/docs/infra/deploy/deploy) and [Source code storage](https://vibecode.bitrix24.com/docs/source-storage).

{% endnote %}

## Handing an App Over to Another Employee {#transfer}

An app can be handed over to any colleague together with the sources. This is needed when the author leaves the company or the person responsible for the app changes.

The handover is started by the app owner or a Bitrix24 administrator: open the action menu in the app card and select the transfer option. The list of recipients contains employees who have already signed in to Vibecode from this Bitrix24. Before confirmation, the dialog shows what will move to the new owner: the keys, the server, the placements, the employees with access, and the source snapshot. The whole bundle moves in a single operation, and the previous owner loses access immediately.

The personal key is reissued during the handover. The new key is substituted into the server variables automatically, while the previous one keeps working for another 24 hours so that the app does not stop. The app authorization key is reissued by the new owner — this requires signing in to Bitrix24 again.

The right to download the sources changes in the same operation. If the storage holds a source snapshot, the new owner receives a message from the Vibecode bot with a button to open the sources and a ready-made prompt for an AI agent. If there is no snapshot, the handover still goes through, but the new owner requests the sources from the previous one.

In the Vibecode dashboard, a handed-over app is marked as transferred to you and shows a panel for completing the handover: what has already moved, what still needs to be configured, and how long the previous key keeps working. There are two ways to take the app over: give the AI agent the prompt from the bot message together with your new key, or reissue the key in the app card.

## What's Next

- [How to Build Your First App](vibecode-create-app.md) — key types, permissions for creating them, prompts, and result verification

- [Vibecode Tools](vibecode-tools.md) — Black Hole servers, source storage, app publication, and other tools

- [Alaio Vibecode](vibecode.md) — platform overview
