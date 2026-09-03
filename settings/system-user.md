# System user

A system user is a technical account. Like a regular employee, it exists at the user level in Bitrix24, but it is not a company employee and is used only for integrations.

Webhooks and applications are linked to a specific employee. If this employee is terminated, integrations may stop working. To avoid this, when an employee is terminated, integrations can be automatically transferred to a system user — they will continue to work without changes.

For applications, a system user can also be created during installation. After the system user is created or reactivated, the application receives the [`ONAPPUSERREADY`](../api-reference/common/events/on-app-user-ready.md) event with this user's authorization.

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
| For applications `RESOURCE_TYPE: APP` | When installing an application or transferring an application from the Marketplace |

When integrations are transferred, the system user inherits the terminated employee's access groups — webhooks and applications continue to work with the same permissions. An application works with a clone of the user whose authorization tokens it used. Permissions are retained, and the transfer is transparent for the application.

When an application is installed, a system user is created once for this application and exists while the application is installed. Bitrix24 takes the time zone, language, and access groups from the employee who installed the application. Elevated roles of the employee who installed the application are not inherited: in the company structure, the system user receives the role of a regular employee.

When the application is deleted, the system user is deactivated. Related integrations are disabled together with it: the application and its webhooks.

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

There is no separate method for retrieving an application system user. The application receives its ID and authorization data from the [`ONAPPUSERREADY`](../api-reference/common/events/on-app-user-ready.md) event.

## Continue Learning

- [{#T}](../local-integrations/local-webhooks.md)
- [{#T}](app-uninstallation.md)
- [{#T}](app-installation/mass-market-apps/index.md)
- [{#T}](../api-reference/common/events/on-app-user-ready.md)
