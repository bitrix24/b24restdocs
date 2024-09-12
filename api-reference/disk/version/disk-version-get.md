# Get Version by ID disk.version.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is absent

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.version.get` returns the version by ID.


## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Version identifier. ||
|#

## Successful Response

> 200 OK

```json
"result": {
    "ID": "2414", //identifier
    "CREATED_BY": "1", //identifier of the user who created the version
    "CREATE_TIME": "2017-05-22T16:32:32+02:00", //creation time
    "DOWNLOAD_URL": "https://test.ivandivan/rest/.../download/?token=...", //link to download the content
    "GLOBAL_CONTENT_VERSION": "1", //incremental version counter relative to the file
    "NAME": "Screenshot from 2017-05-19 09-13-02.png", //file name at the time of version creation
    "OBJECT_ID": "8933", //identifier of the file to which the version belongs
    "SIZE": "39078" //size of the version in bytes
}
```