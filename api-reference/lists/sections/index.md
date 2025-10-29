# Working with Sections: Overview of Methods

Sections help organize data and simplify navigation within lists. They group records by categories or levels of nesting.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to create and configure a list in Bitrix24](https://helpdesk.bitrix24.com/open/25744043/)

## Connection with Other Objects

**Universal lists.** Each section belongs to a specific list. To add a section to a list, you need to specify the parameters `IBLOCK_TYPE_ID`, `IBLOCK_ID`, and `IBLOCK_CODE`. Values can be obtained using the [lists.get](../lists/lists-get.md) method.

{% note tip "User documentation" %}

- [List view settings](https://helpdesk.bitrix24.com/open/18128266/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [lists.section.add](./lists-section-add.md) | Creates a list section ||
|| [lists.section.update](./lists-section-update.md) | Updates a list section ||
|| [lists.section.get](./lists-section-get.md) | Returns a section or a list of sections ||
|| [lists.section.delete](./lists-section-delete.md) | Deletes a list section ||
|#