# Restore a file from a specific version disk.file.restoreFromVersion

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no error response provided

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.file.restoreFromVersion` restores a file from a specific version.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Folder identifier. ||
|| **versionId**
[`unknown`](../../data-types.md) | Version identifier. ||
|#

## Successful response

> 200 OK

The response will return the updated file, and the response structure is similar to [disk.file.get](./disk-file-get.md).