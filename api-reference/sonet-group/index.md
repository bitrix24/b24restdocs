# About Workgroups and Projects

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- what it is, where it is located, how the project differs from a workgroup, how to distinguish a simple project from a Scrum project

{% endnote %}

{% endif %}

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [socialnetwork.api.workgroup.get](./socialnetwork-api-workgroup-get.md) | Retrieves data for a workgroup ||
    || [socialnetwork.api.workgroup.list](./socialnetwork-api-workgroup-list.md) | Retrieves a list of workgroups ||
    || [sonet_group.create](./sonet-group-create.md) | Creates a social network group ||
    || [sonet_group.update](./sonet-group-update.md) | Modifies the parameters of a social network group ||
    || [sonet_group.get](./sonet-group-get.md) | Retrieves a list of social network groups ||
    || [sonet_group.delete](./sonet-group-delete.md) | Deletes a social network group ||
    || [sonet_group.setowner](./sonet-group-setowner.md) | Changes the owner of the group ||
    || [sonet_group.feature.access](./sonet-group-feature-access.md) | Checks the rights of the current user ||
    || [sonet_group.user.groups](./sonet-group-user-groups.md) | Retrieves a list of the current user's groups ||
    |#

- Events

    #| 
    || **Event** | **Description** ||
    || [onSonetGroupAdd](./events/on-sonet-group-add.md) | Triggered after a new workgroup is added. Proxy to the event `OnSocNetGroupAdd` ||
    || [onSonetGroupDelete](./events/on-sonet-group-delete.md) | Triggered at the moment a workgroup is deleted. Proxy to the event `OnSocNetGroupDelete` ||
    || [onSonetGroupSubjectAdd](./events/on-sonet-group-subject-add.md) | Triggered after a workgroup topic is created. Proxy to the event `OnSocNetGroupSubjectAdd` ||
    || [onSonetGroupSubjectDelete](./events/on-sonet-group-subject-delete.md) | Triggered before a workgroup topic is deleted. Proxy to the event `OnSocNetGroupSubjectDelete` ||
    || [onSonetGroupSubjectUpdate](./events/on-sonet-group-subject-update.md) | Triggered after a workgroup topic is modified. Proxy to the event `OnSocNetGroupSubjectUpdate` ||
    || [onSonetGroupUpdate](./events/on-sonet-group-update.md) | Triggered after a workgroup is modified. Proxy to the event `OnSocNetGroupUpdate` ||
    |#

{% endlist %}

## Participants

#| 
|| [sonet_group.user.add](./members/sonet-group-user-add.md) | Adds users to the group ||
|| [sonet_group.user.delete](./members/sonet-group-user-delete.md) | Removes users from the group ||
|| [participants](./members/sonet-group-user-get-expanded.md) | Retrieves an expanded list ||
|| [sonet_group.user.get](./members/sonet-group-user-get.md) | Retrieves a list of group participants ||
|| [sonet_group.user.invite](./members/sonet-group-user-invite.md) | Invites users to the group ||
|| [sonet_group.user.request](./members/sonet-group-user-request.md) | Sends a request to join the group ||
|| [sonet_group.user.update](./members/sonet-group-user-update.md) | Modifies the user's role in the group ||
|#