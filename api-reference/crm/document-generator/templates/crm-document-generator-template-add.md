# Add a new template crm.documentgenerator.template.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.template.add` adds a new template. It returns the same data as when calling [crm.documentgenerator.template.get()](./crm-document-generator-template-get.md) on the new template.

#| 
|| **Parameter** | **Description** ||
|| **fields** | Array of template fields. ||
|#

## Parameter fields

#| 
|| **Parameter** | **Description** ||
|| **name**^*^ | Template name. ||
|| **file**^*^ | File content, encoded in [base64](../../../files/how-to-upload-files.md). Alternatively, the file content can be sent in `multipart / form-data`. In this case, it does not need to be encoded in `base64`. ||
|| **numeratorId**^*^ | Identifier of the numerator. ||
|| **region**^*^ | Country. ||
|| **entityTypeId**^*^ | Array of identifiers of linked entities. The deal code must be passed here, considering filtering by directions. ||
|| **users** | Array of access permissions. ||
|| **active** | Y/N active flag. ||
|| **withStamps** | Y/N to apply stamps and signatures. ||
|| **sort** | Sorting index. ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}