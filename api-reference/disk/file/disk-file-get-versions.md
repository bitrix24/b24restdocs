# List of File Versions disk.file.getVersions

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no response in case of an error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.file.getVersions` returns a list of file versions. The versions are sorted in descending order by creation date.

## Parameters

#|
||  **Parameter** / **Type**| **Description** | **Available since** ||
|| **id**
[`unknown`](../../data-types.md) | File identifier | ||
|| **filter**
[`unknown`](../../data-types.md) | Optional parameter. Supports filtering by fields specified in `disk.version.getfields` as `USE_IN_FILTER: true`. | ||
|#

{% note info %}

See also the description of [list methods](../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

{% endnote %}

## Successful Response

> 200 OK

The response contains a list of versions, the structure of which is similar to [disk.version.get](../version/disk-version-get.md).