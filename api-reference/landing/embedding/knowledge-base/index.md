# Embedding Knowledge Base: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Knowledge Base can be embedded into the Bitrix24 interface in two ways: by displaying it in the menu or linking it to a group. This will help employees find the necessary articles and instructions more quickly.

For example, the department's Knowledge Base can be linked to a group so that employees can read instructions and regulations within the group's workspace. If the Knowledge Base is needed by all employees, it can be displayed in a specific menu.

> Quick navigation: [all methods](#all-methods)

## How to Manage Knowledge Base Embedding

The Knowledge Base can be embedded into the Bitrix24 interface and managed accordingly. There are two main options: add the Knowledge Base to the menu or link it to a group.

**Linking to the Menu.** This option is suitable if the Knowledge Base needs to be accessible from the same location in the interface. For example, the Knowledge Base with sales scripts can be added to the deals list. This way, the manager can access it from the deals section without switching to another area.

To set up the link:

1. Obtain the site ID of the Knowledge Base using the [landing.site.getList](../../site/landing-site-get-list.md) method.
2. Define `menuCode` — the code of the location in the interface where the Knowledge Base should be added. Instructions on how to find the code are described in the parameters of the [landing.site.bindingToMenu](./landing-site-binding-to-menu.md) method.
3. Execute the link using the [landing.site.bindingToMenu](./landing-site-binding-to-menu.md) method.
4. Check the result using the [landing.site.getMenuBindings](./landing-site-get-menu-bindings.md) method. If the method returns `false`, check the bindings — the Knowledge Base may already be linked to the menu.
5. If the link is no longer needed, it can be removed using the [landing.site.unbindingFromMenu](./landing-site-unbinding-from-menu.md) method.

**Linking to a Group.** This option is suitable if the Knowledge Base is only needed by members of a specific group. It can be used for departments that have their own instructions, templates, and regulations.

For example, the support service's Knowledge Base can be linked to the department's group. This way, employees can read instructions and response templates within the group's workspace without searching through other sections.

To set up the link:

1. Obtain the site ID of the Knowledge Base using the [landing.site.getList](../../site/landing-site-get-list.md) method.
2. Get the `groupId` of the group from the group interface or using the [socialnetwork.api.workgroup.list](../../../sonet-group/socialnetwork-api-workgroup-list.md) or [sonet_group.get](../../../sonet-group/sonet-group-get.md) methods.
3. Execute the link using the [landing.site.bindingToGroup](./landing-site-binding-to-group.md) method.
4. Check the result using the [landing.site.getGroupBindings](./landing-site-get-group-bindings.md) method. If the method returns `false`, check the bindings: only one Knowledge Base can be linked to a single group.
5. If the link is no longer needed, it can be removed using the [landing.site.unbindingFromGroup](./landing-site-unbinding-from-group.md) method.

## Connection with Other Objects

**Site.** The Knowledge Base is represented as a separate site in the `landing` module. The site ID `id` is needed for the linking and unlinking methods.

**Interface Menu.** The `menuCode` parameter is used for linking to the menu. It determines where in the interface the Knowledge Base will be accessible.

**Group.** The `groupId` parameter is used for linking to a group. It specifies which group the Knowledge Base will be associated with.

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

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