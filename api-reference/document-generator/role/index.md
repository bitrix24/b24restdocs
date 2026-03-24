# Roles in the Document Generator: Overview of Methods

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with permission to modify the document generator settings

Roles define access to settings, templates, and documents in the Document Generator.

In the request parameter `permissions` for the methods [documentgenerator.role.add](./document-generator-role-add.md) and [documentgenerator.role.update](./document-generator-role-update.md), use the keys in uppercase:
- `SETTINGS.MODIFY`
- `TEMPLATES.MODIFY`
- `DOCUMENTS.MODIFY`
- `DOCUMENTS.VIEW`

In the response of the method `documentgenerator.role.get`, these same parameters are returned in lowercase:
- `settings.modify`
- `templates.modify`
- `documents.modify`
- `documents.view`

Access levels:
- `""` — no access
- `A` — only their own items
- `D` — their own and colleagues in the department
- `X` — full access

Levels `A` and `D` apply only to `templates.modify`. For other actions, `""` or `X` is used.

Binding a role to users and groups is done using the method `documentgenerator.role.fillaccesses` through the `accesses` array with access codes `accessCode`. For example:
- `U1` — user with ID `1`
- `D1` — department with ID `1`
- `UA` — all authorized users

#|
|| **Method** | **Description** ||
|| [documentgenerator.role.add](./document-generator-role-add.md) | Adds a role and returns its data with permissions ||
|| [documentgenerator.role.update](./document-generator-role-update.md) | Updates a role and returns the current role data ||
|| [documentgenerator.role.get](./document-generator-role-get.md) | Returns a role by identifier along with permissions ||
|| [documentgenerator.role.list](./document-generator-role-list.md) | Returns a list of roles without detailed permission composition ||
|| [documentgenerator.role.delete](./document-generator-role-delete.md) | Deletes a role by identifier ||
|| [documentgenerator.role.fillaccesses](./document-generator-role-fill-accesses.md) | Completely overwrites role bindings to access codes ||
|#