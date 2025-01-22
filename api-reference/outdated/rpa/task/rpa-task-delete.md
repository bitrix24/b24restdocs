# Remove Automation Rule from Process rpa.task.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- required parameters are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.task.delete` will remove the Automation rule with the name robotName from the process with the identifier typeId at the stage with the identifier stageId.

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **stageId** 
[`number`](../../../data-types.md) | Identifier of the stage. ||
|| **robotName** 
[`string`](../../../data-types.md) | Name of the Automation rule. ||
|#