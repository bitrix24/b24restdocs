# System user

A system user is a technical account. Like a regular employee, it exists at the user level in Bitrix24, but it is not a company employee and is used only for integrations.

This page describes when Bitrix24 creates a system user, how the webhooks of a dismissed employee are transferred to it, how an application receives its authorization, and how to disable the integrations associated with it.

## System user types

| Type | When created | Purpose |
|---|---|---|
| For webhooks `RESOURCE_TYPE: WEBHOOK` | When an employee is dismissed, if the administrator kept their inbound webhooks | Webhooks keep working after the employee is dismissed |
| For applications `RESOURCE_TYPE: APP` | When a mass-market application is installed | The application works without the involvement of the employee who installed it |

## How webhook transfer works {#webhooks}

If an employee has inbound webhooks, a selection dialog opens on dismissal. The administrator who dismisses the employee selects one of two options:

- *Disable solutions and dismiss user* — the webhooks stop working
- *Keep solutions and dismiss user* — a system user of the `RESOURCE_TYPE: WEBHOOK` type is created, and the employee's webhooks are transferred to it

The system user receives the first name, last name, and access groups of the dismissed employee, so the webhooks keep working with the same permissions. Webhook URLs do not change: Bitrix24 executes a request to the old URL on behalf of the system user.

A kept webhook remains usable by anyone who knows its URL, including the dismissed employee. If the webhook is no longer needed, select *Disable solutions and dismiss user* or delete the webhook.

{% note info "" %}

If an employee edits a webhook operating on behalf of a system user, the webhook is automatically transferred to that employee.

{% endnote %}

## Application system user {#app}

An application system user lets a mass-market application work in the background without the involvement of an employee. Its authorization does not depend on the employee who installed the application and keeps working after that employee is dismissed.

### When it is created

When you install an application, clicking *Install* opens a window with the *Create system user* checkbox:

- checkbox selected — Bitrix24 creates a system user and sends the application the [`ONAPPUSERREADY`](../api-reference/common/events/on-app-user-ready.md) event with its authorization
- checkbox cleared — the system user is not created and the `ONAPPUSERREADY` event is not sent. The `ONAPPINSTALL` event is sent as usual

The choice is retained on the first installation of the application. Applications installed before the checkbox appeared receive a system user on update, as before.

A system user is not created:

- for [local applications](../local-integrations/local-apps.md)
- when installing via `CRestUtil::InstallApp`

A system user is created once for the application and exists while the application is installed. If the application already has one, for example after reinstallation, Bitrix24 reactivates it and sends `ONAPPUSERREADY` again.

### Which permissions it receives

Bitrix24 takes the time zone, language, and access groups from the employee who installed the application. Elevated roles of the employee who installed the application are not inherited: in the company structure, the system user receives the role of a regular employee.

### How the application receives authorization

There is no separate method for retrieving an application system user. The application receives its ID and tokens from the `data` object of the `ONAPPUSERREADY` event. Retain these tokens separately from the employee tokens that arrive in `ONAPPINSTALL`.

### What happens when an employee is dismissed

Applications are not transferred to a system user when an employee is dismissed, and the selection dialog does not show them. The tokens the application received on behalf of the dismissed employee are not passed to the system user. To keep the application working, use the authorization from `ONAPPUSERREADY` for background calls.

### What happens when the application is deleted

When the application is deleted, the system user is deactivated, and its authorization stops working.

## Parameters of the created user

| Field | Type | For a webhook | For an application |
|---|---|---|---|
| `NAME` | string | First name of the dismissed employee | Application name |
| `LAST_NAME` | string | Last name of the dismissed employee | Empty string |
| `GROUP_ID` | array | Inherited from the dismissed employee | Inherited from the employee who installed the application |
| `TIME_ZONE` | string | Inherited from the dismissed employee | Inherited from the employee who installed the application |
| `LANGUAGE_ID` | string | Inherited from the dismissed employee | Inherited from the employee who installed the application |
| `EXTERNAL_AUTH_ID` | string | `rest_system` | `rest_system` |
| `ACTIVE` | boolean | `true` | `true` |

## How to find a system user

A system user is a technical object. By default, it is hidden in the employee list and cannot be found via search in Bitrix24. To see system users, select the *System users* filter preset in the employee list.

The `user.get` and `user.search` methods do not return a system user, including when filtering by the `EXTERNAL_AUTH_ID` field.

## How to disable system user integrations

A system user can be dismissed from its profile or with a group action in the employee list with the *System users* preset. The dismissal dialog shows the application and inbound webhooks associated with it. After you confirm with *Disable integrations and dismiss*, they are disabled and stop working on behalf of the system user.

## Continue Learning

- [{#T}](../local-integrations/local-webhooks.md)
- [{#T}](app-uninstallation.md)
- [{#T}](app-installation/mass-market-apps/index.md)
- [{#T}](../api-reference/common/events/on-app-user-ready.md)
