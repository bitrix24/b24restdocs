# Update Stage rpa.stage.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.stage.update` will update the stage with the given id and return data in the response similar to the response from the `rpa.stage.get` request.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the stage. ||
|| **fields** 
[`array`](../../../data-types.md) | List of stage fields. ||
|#

## fields Parameters

#|
|| **Parameter** | **Description** ||
|| **name** | Name of the stage. ||
|| **code** | Symbolic code. ||
|| **color** | Color of the stage in HEX format (6 characters). ||
|| **sort** | Sort identifier. ||
|| **semantic** | Semantic code of the stage. It can be either SUCCESS or FAIL. ||
|#

{% note warning %}

There must always be one successful stage in the process. The semantics of the successful stage cannot be changed.

{% endnote %}