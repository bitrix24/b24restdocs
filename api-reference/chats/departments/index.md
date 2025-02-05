# Working with Departments

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no content (only a list of methods is available)

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The methods `im.department.*` are available only for [intranet users](*intranet_users).

#|
|| **Method** | **Description** ||
|| [im.department.colleagues.list](./im-department-colleagues-list.md) | Retrieves a list of users in your department ||
|| [im.department.employees.get](./im-department-employees-get.md) | Retrieves a list of department employees ||
|| [im.department.get](./im-department-get.md) | Retrieves information about the department ||
|| [im.department.managers.get](./im-department-managers-get.md) | Retrieves a list of department managers ||
|#

[*intranet_users]: In Bitrix24, there are two types of users:
- intranet users – internal users (employees of your company);
- extranet users – external users (vendors, distributors, etc.).
[Learn more...](https://helpdesk.bitrix24.com/open/18070866/)