# Create a New CRM Status Element: crm.status.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (in other languages)
- success response is missing
- error response is missing
- parameter types are not specified

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.status.add(fields)
```

This method creates a new element in the specified directory.

{% note info "" %}

If a stage is added for a custom deal direction, a prefix corresponding to the direction will be automatically added to the status ID. This is necessary to identify the direction by the stage ID.

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
| A set of fields - an array of the form `array("field"=>"value"[, ...])`, containing the values of the directory fields. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

**Attention!**

Starting from CRM module version 20.5.500, there is a restriction on the length and format of the STATUS_ID field value for certain ENTITY_IDs:

- **STATUS** (lead status). Max length: 21, can contain only Latin letters, digits, hyphens, and underscores.
- **QUOTE_STATUS** (invoice status). Max length: 22, can contain only Latin letters, digits, hyphens, and underscores.
- **DEAL_STAGE** (deal status). Max length: 22, can contain only Latin letters, digits, hyphens, and underscores.
- **DEAL_STAGE_xx** (deal status in non-default directions. xx - direction identifier). Max length depends on the direction identifier. Can contain only Latin letters, digits, hyphens, and underscores.
- For other ENTITY_IDs, the maximum length of STATUS_ID is 50 characters, and it can contain any characters.

## Examples

```javascript
BX24.callMethod(
    "crm.status.add",
    {
        fields:
        {
            "ENTITY_ID": "DEAL_STAGE",        
            "STATUS_ID": "DECISION",
            "NAME": "Decision Making",
            "SORT": 70
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info("Created directory element with ID " + result.data());
    }
);
```

```javascript
BX24.callMethod(
    "crm.status.add",
    {
     fields:
     {
         "ENTITY_ID": "DEAL_STAGE_1",        
         "STATUS_ID": "DECISION",
         "NAME": "Decision Making",
         "SORT": 70
     }
},
function(result)
{
     if(result.error())
         console.error(result.error());
     else
         console.info("Created directory element with ID " + result.data());
}
);
```
In the second example, the STATUS_ID field will be saved as `C1:DECISION`, where the prefix "C1:" corresponding to the deal direction identifier will be added.

{% include [Example Note](../../../_includes/examples.md) %}