# Delete User Field Settings userfieldconfig.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- no example provided
- missing success response
- missing error response
  
{% endnote %}

{% endif %}

> Scope: [`userfieldconfig, module scope`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
userfieldconfig.delete({moduleId: string, id: number})
```

This method will delete the field settings with the identifier id.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **moduleId^*^** | String identifier of the module.  | ||
|| **id^*^** | Identifier of the field settings.  | ||
|#

{% include [Parameter Notes](../../../../../_includes/required.md) %}