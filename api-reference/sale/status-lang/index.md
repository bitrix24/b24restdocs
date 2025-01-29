# Localization of Order and Delivery Statuses in Online Stores: Overview of Methods

[Order and Delivery Statuses](../status/index.md) can be adapted for different languages.

> Quick navigation: [all methods](#all-methods)

## Localization Languages

Status localization is available for the following languages:
- `ar` — Arabic,
- `br` — Portuguese,
- `de` — German,
- `en` — English,
- `fr` — French,
- `hi` — Hindi,
- `id` — Indonesian,
- `in` — English for India,
- `it` — Italian,
- `ja` — Japanese,
- `la` — Spanish,
- `ms` — Malay,
- `pl` — Polish,
- `de` — German,
- `sc` — Simplified Chinese,
- `tc` — Traditional Chinese,
- `th` — Thai,
- `tr` — Turkish,
- `ua` — Ukrainian,
- `vn` — Vietnamese.

The list of current languages can be obtained using the method [sale.statuslang.getlistlangs](./sale-status-lang-get-list-langs.md).

## Connection of Status Localization with Other Objects

**Status.** Specify the identifier of the required status. The list of identifiers can be obtained using the method [sale.status.list](../status/sale-status-list.md).

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [sale.statusLang.getListLangs](./sale-status-lang-get-list-langs.md) | Returns a list of languages for localizing the order or delivery status ||
|| [sale.statusLang.add](./sale-status-lang-add.md) | Adds localization for a status ||
|| [sale.statusLang.list](./sale-status-lang-list.md) | Returns a list of status localizations ||
|| [sale.statusLang.deleteByFilter](./sale-status-lang-delete-by-filter.md) | Deletes localization for a status ||
|| [sale.statusLang.getFields](./sale-status-lang-get-fields.md) | Returns fields for status localization ||
|#