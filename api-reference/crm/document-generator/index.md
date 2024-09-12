# Document Generator

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- An introduction corresponding to the title is needed.

{% endnote %}

{% endif %}

{% note info "Permissions" %}

**Scope**: [`crm.documentgenerator`](../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The CRM REST methods for documents are implemented through AJAX controllers. The controllers and their actions are implemented in the [Document Generator](https://training.bitrix24.com/api_d7/bitrix/documentgenerator/index.php) module, while the CRM module only provides a wrapper for them. This wrapper transforms the input and output parameters according to the new REST standard, taking into account the specifics of the CRM module.

When working with document methods through the CRM scope, you can only work with templates/documents linked to the CRM module.

## Features of Passed Values

### Providers

The **Document Generator** module uses the full class name as the data provider identifier. Since the CRM module uses named/numeric identifiers to identify entities, a similar syntax was used in REST for documents. If the input parameter is called *entityTypeId*, it accepts the numeric index of CRM entities. Currently, the following identifiers are available:

- 1 - lead
- 2 - deal
- 3 - contact
- 4 - company
- 5 - invoice
- 7 - estimate
- 14 - order
- 31 - new invoices

It is also possible to use SPAs: their *entityTypeId* is also supported in the Document Generator.

### Filtering by Deal Directions

REST methods do not consider the template binding settings to providers. That is, if a request is sent to generate a document based on a template with a provider to which this template is not linked, there will be no error. It follows that the binding of the template to a specific deal direction is not taken into account. If you want to specify a deal as a provider, only the numeric identifier (2) is always indicated.

### List of Regions

Each template is linked to a specific country. The list of countries is fixed and currently consists of:

- com - Russia
- com - Belarus
- com - Kazakhstan
- com - Ukraine
- com - Brazil
- com - Mexico
- com - Germany
- com - United Kingdom
- com - Poland

Region management is carried out in the [documentgenerator](../../document-generator/region/index.md) section.