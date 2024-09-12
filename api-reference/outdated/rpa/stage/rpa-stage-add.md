# Create a New Stage rpa.stage.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.stage.add` will create a new stage and return data in the response similar to the response from the `rpa.stage.get` request.

#|
|| **Parameter** / **Type** | **Description** ||
|| **fields**
[`array`](../../../data-types.md) | A list of stage fields. The list of possible fields is described below.||
|#

## Fields Parameters

#|
|| **Parameter** | **Description** ||
|| **name**^*^ | The name of the stage. ||
|| **typeId**^*^ | The identifier of the process. ||
|| **code** | Symbolic code. ||
|| **color** | The color of the stage in HEX format (6 characters). ||
|| **sort** | The sorting identifier. ||
|| **semantic** | The semantic code of the stage. It can be either SUCCESS or FAIL. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

{% note warning %}

A process can have only one successful stage.

{% endnote %}