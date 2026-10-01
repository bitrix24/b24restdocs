# Embedding Knowledge Base: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The Knowledge Base can be embedded into the Bitrix24 interface in two ways: by displaying it in the menu or linking it to a group.

Other embedding locations in the Sites section are registered with the [landing.repo.bind](../landing-repo-bind.md) method and are described in the [Embedding Locations in the Sites Section](../index.md) overview. The Knowledge Base is bound with the separate methods listed below because the `landing` module represents it as a separate site.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Add a knowledge base to a section](https://helpdesk.bitrix24.com/open/19427378/)

## How to Manage Knowledge Base Embedding

**Linking to the Menu.** This option is suitable if the Knowledge Base needs to be accessible from the same location in the interface. For example, the Knowledge Base with sales scripts can be added to the deals list. This way, the manager can access it from the deals section without switching to another area.

To set up the link:

1. Retrieve the Knowledge Base site ID with the [landing.site.getList](../../site/landing-site-get-list.md#type-scope) method and the `scope: "KNOWLEDGE"` parameter.
2. Determine the menu code `menuCode`, for example `crm_switcher:deal`. How to retrieve it is described in the parameters of the [landing.site.bindingToMenu](./landing-site-binding-to-menu.md) method.
3. Execute the link using the [landing.site.bindingToMenu](./landing-site-binding-to-menu.md) method.
4. Check the result with the [landing.site.getMenuBindings](./landing-site-get-menu-bindings.md) method and the same `menuCode`: the binding should appear in the list.
5. Remove the binding with the [landing.site.unbindingFromMenu](./landing-site-unbinding-from-menu.md) method if it is no longer needed.

**Linking to a Group.** This option is suitable if the Knowledge Base is only needed by members of a specific group. For example, a department's Knowledge Base can be linked to the department's group so that employees can read instructions and regulations within the group's workspace. Only one Knowledge Base can be linked to a group.

To set up the link:

1. Retrieve the Knowledge Base site ID with the [landing.site.getList](../../site/landing-site-get-list.md#type-scope) method and the `scope: "KNOWLEDGE"` parameter.
2. Retrieve the group ID `groupId` with the [socialnetwork.api.workgroup.list](../../../sonet-group/socialnetwork-api-workgroup-list.md) or [sonet_group.get](../../../sonet-group/sonet-group-get.md) method. It is also shown in the group interface.
3. Execute the link using the [landing.site.bindingToGroup](./landing-site-binding-to-group.md) method.
4. Check the result with the [landing.site.getGroupBindings](./landing-site-get-group-bindings.md) method and the same `groupId`: the binding should appear in the list.
5. Remove the binding with the [landing.site.unbindingFromGroup](./landing-site-unbinding-from-group.md) method if it is no longer needed.

The binding and unbinding methods do not return an error when the action is not performed: they respond with `false`. The reasons are listed on the method pages.

## Relationships with Other Objects

The Knowledge Base is related to sites of the `landing` module, menus, and Bitrix24 groups.

**Site.** A Knowledge Base is a `landing` module site of the `KNOWLEDGE` type. How the site type affects the `scope` parameter in site methods is described in [Working with Site Types and Scopes](../../types.md).

**Group.** When bound to a group, the Knowledge Base gets the `GROUP` type, and site methods find it only with `scope: "GROUP"`. After unbinding, the type changes back to `KNOWLEDGE`. That is why the `id` for [landing.site.unbindingFromGroup](./landing-site-unbinding-from-group.md) is taken from the [landing.site.getGroupBindings](./landing-site-get-group-bindings.md) response.

**Menu.** The `menuCode` parameter sets the location in the interface. One Knowledge Base can be bound to several menus: call [landing.site.bindingToMenu](./landing-site-binding-to-menu.md) with a separate `menuCode` for each.

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

### Linking to the Menu

#| 
|| **Method** | **Description** ||
|| [landing.site.bindingToMenu](./landing-site-binding-to-menu.md) | Links the Knowledge Base to the menu ||
|| [landing.site.getMenuBindings](./landing-site-get-menu-bindings.md) | Retrieves the Knowledge Base's menu bindings ||
|| [landing.site.unbindingFromMenu](./landing-site-unbinding-from-menu.md) | Unlinks the Knowledge Base from the menu ||
|#

### Linking to a Group

#| 
|| **Method** | **Description** ||
|| [landing.site.bindingToGroup](./landing-site-binding-to-group.md) | Links the Knowledge Base to a group ||
|| [landing.site.getGroupBindings](./landing-site-get-group-bindings.md) | Retrieves the Knowledge Base's group bindings ||
|| [landing.site.unbindingFromGroup](./landing-site-unbinding-from-group.md) | Unlinks the Knowledge Base from a group ||
|#
