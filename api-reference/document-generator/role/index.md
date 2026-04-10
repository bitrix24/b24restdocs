# Roles in the Document Generator: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Roles define access permissions to settings, templates, and documents in the Document Generator.

> Quick Navigation: [all methods](#all-methods)

## Getting Started

1. Retrieve the list of roles using the [documentgenerator.role.list](./document-generator-role-list.md) method.
2. If the required role is not available, create it using the [documentgenerator.role.add](./document-generator-role-add.md) method.
3. Obtain the role's permissions composition using the [documentgenerator.role.get](./document-generator-role-get.md) method.
4. Assign the role to users, departments, or working groups using the [documentgenerator.role.fillaccesses](./document-generator-role-fill-accesses.md) method.
5. If necessary, modify the role's permissions using the [documentgenerator.role.update](./document-generator-role-update.md) method.
6. If the role is no longer needed, delete it using the [documentgenerator.role.delete](./document-generator-role-delete.md) method.

{% note info "" %}

The [documentgenerator.role.list](./document-generator-role-list.md) method does not return detailed permissions for each role. Use it to get a list of roles and select the required `id`. For detailed role information and the `permissions` object, use the [documentgenerator.role.get](./document-generator-role-get.md) method.

{% endnote %}

## What Permissions Does a Role Define

In the `permissions` parameter, the role's permissions are grouped into three blocks:

- `SETTINGS` — for modifying the document generator settings,
- `TEMPLATES` — for working with templates,
- `DOCUMENTS` — for modifying and viewing documents.

Within these blocks, the action keys `MODIFY` and `VIEW` are used.

In the [documentgenerator.role.add](./document-generator-role-add.md) and [documentgenerator.role.update](./document-generator-role-update.md) methods, use the permission keys in uppercase, for example, `SETTINGS.MODIFY`.

```js
permissions: {
    SETTINGS: {
        MODIFY: ''
    },
    TEMPLATES: {
        MODIFY: 'D'
    },
    DOCUMENTS: {
        MODIFY: 'X',
        VIEW: 'X'
    }
}
```

In the response from the [documentgenerator.role.get](./document-generator-role-get.md) method, these same permissions are returned in lowercase, for example, `settings.modify`.

```json
"permissions": {
    "settings": {
        "modify": ""
    },
    "templates": {
        "modify": "D"
    },
    "documents": {
        "modify": "X",
        "view": "X"
    }
}
```

The following access levels are used for permissions:
- `""` — no access
- `A` — only own entities
- `D` — own and department colleagues' entities
- `X` — full access

Levels `A` and `D` apply only to `templates.modify`. For other actions, use `""` or `X`.

## Relationship with Other Objects

**Document Templates.** Role permissions affect the work with templates. If the application needs to create, modify, or view templates, configure the appropriate role. Then use the [documentgenerator.template.*](../templates/index.md) methods.

**Documents.** Role permissions define access to documents and methods for working with documents [documentgenerator.document.*](../index.md). Document permissions are necessary for the application if it creates or reads documents.

**Document Generator Settings.** The `SETTINGS.MODIFY` permission relates to the administrative settings of the Document Generator module. If a user does not need to change module settings, this permission can be omitted even with access to templates and documents.

## How to Assign Roles to Users and Groups

Role binding is performed using the [documentgenerator.role.fillaccesses](./document-generator-role-fill-accesses.md) method. In the `accesses` array, the following are passed for each binding:

- `roleId` — role identifier
- `accessCode` — code for the user, department, group, or other access group

Examples of access codes:

- `U1` — user with identifier `1`
- `D1` — department with identifier `1`
- `AU` — all authorized users

For a complete list of access codes, refer to the description of the [method](./document-generator-role-fill-accesses.md).

The [documentgenerator.role.fillaccesses](./document-generator-role-fill-accesses.md) method completely overwrites the role's bindings to access codes. If you need to preserve current bindings:

1. Retrieve the list of roles using the [documentgenerator.role.list](./document-generator-role-list.md) method and select the required `id`.
2. Check the role's permissions using the [documentgenerator.role.get](./document-generator-role-get.md) method.
3. Prepare a complete list of new bindings and send it to [documentgenerator.role.fillaccesses](./document-generator-role-fill-accesses.md).
4. After making changes, verify the role and associated access scenarios again.

## What to Consider When Changing Permissions

If incomplete or incorrect data is passed in `accesses`, the method may return `null`, just like in a successful execution. You need to additionally ensure that correct `roleId` and `accessCode` were sent, and then check the actual result in the access workflow.

Changing the role itself is done using the [documentgenerator.role.update](./document-generator-role-update.md) method. It updates the name, code, and permissions of the role. Assigning this role to users and groups is modified using the [documentgenerator.role.fillaccesses](./document-generator-role-fill-accesses.md) method.

## Overview of Methods {#all-methods}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with permission to modify document generator settings

#|
|| **Method** | **Description** ||
|| [documentgenerator.role.add](./document-generator-role-add.md) | Adds a role and returns its data with permissions ||
|| [documentgenerator.role.update](./document-generator-role-update.md) | Updates a role and returns the current role data ||
|| [documentgenerator.role.get](./document-generator-role-get.md) | Returns a role by identifier along with permissions ||
|| [documentgenerator.role.list](./document-generator-role-list.md) | Returns a list of roles without detailed permissions ||
|| [documentgenerator.role.delete](./document-generator-role-delete.md) | Deletes a role by identifier ||
|| [documentgenerator.role.fillaccesses](./document-generator-role-fill-accesses.md) | Completely overwrites role bindings to access codes ||
|#