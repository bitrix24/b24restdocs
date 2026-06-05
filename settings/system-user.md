# System user

A system user is a technical account. Like a regular employee, it exists at the user level in Bitrix24, but it is not a company employee and is used only for integrations.

Webhooks and applications are linked to a specific employee. If this employee is terminated, integrations may stop working. To avoid this, when an employee is terminated, integrations can be automatically transferred to a system user — they will continue to work without changes.

## How the transfer works

When an employee with active webhooks or applications is terminated, a selection dialog opens. An Administrator selects one of two options:

- **Terminate and disable integrations** — webhooks and applications stop working.
- **Terminate and preserve integrations** — a system user is created, and all active webhooks and applications are transferred to it.

{% note info "" %}

If an employee edits a webhook operating on behalf of a system user, the webhook is automatically transferred to that employee.

{% endnote %}

## System user types

There are two types of system users.

| Type | When created |
|---|---|
| For webhooks `RESOURCE_TYPE: WEBHOOK` | When transferring webhooks of a terminated employee |
| For applications `RESOURCE_TYPE: APP` | When transferring an application from the Marketplace |

A system user inherits the access groups of the terminated employee — webhooks and applications continue to work with the same permissions. An application works with a clone of the user whose authorization tokens it used. Permissions are preserved, and the transfer is transparent for the application.

{% note tip "" %}

For local applications, transfer to a system user is not supported.

{% endnote %}

### Parameters of the created user

| Field | Type | For a webhook | For an application |
|---|---|---|---|
| `NAME` | string | First name of the terminated employee | Application name |
| `LAST_NAME` | string | Last name of the terminated employee | Empty string |
| `GROUP_ID` | array | Inherited from the original | Inherited from the original |
| `TIME_ZONE` | string | Inherited from the original | Inherited from the original |
| `LANGUAGE_ID` | string | Inherited from the original | Inherited from the original |
| `EXTERNAL_AUTH_ID` | string | `rest_system` | `rest_system` |
| `ACTIVE` | boolean | `true` | `true` |

## System user visibility

A system user is a technical object. It does not appear in the employee list and cannot be found via search in Bitrix24.

The `user.get` and `user.search` methods do not return a system user, including when filtering by the `EXTERNAL_AUTH_ID` field.

## Continue Learning

- [{#T}](../local-integrations/local-webhooks.md)
- [{#T}](app-uninstallation.md)
- [{#T}](app-installation/mass-market-apps/index.md)