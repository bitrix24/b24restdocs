# How Roles Work in the Document Generator

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- Write an article, as it is completely unclear how roles work in the document generator.

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [documentgenerator.role.get](./document-generator-role-get.md) | Returns information about the role and its access permissions. ||
|| [documentgenerator.role.list](./document-generator-role-list.md) | This method will return a list of roles without their access permissions. ||
|| [documentgenerator.role.delete](./document-generator-role-delete.md) | Deletes a role. ||
|| [documentgenerator.role.add](./document-generator-role-add.md) | This method will add a new role. ||
|| [documentgenerator.role.update](./document-generator-role-update.md) | Updates a role. ||
|| [documentgenerator.role.fillaccesses](./document-generator-role-fill-accesses.md) | This method will set a set of roles and their bindings. ||
|#