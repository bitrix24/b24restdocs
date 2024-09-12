# Overview of CRM

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

[CRM](https://helpdesk.bitrix24.com/open/21793278/) is one of the most functional tools in Bitrix. To effectively use the REST API, it is highly recommended to study the product's functionality and key user scenarios. Understanding the relationships between CRM entities will simplify the search and use of the REST API tools needed to implement the desired functionality.

The current section of the documentation includes the main sections:

- [Universal Methods](./universal/index.md)
- [Deals](./deals/index.md)
- [Leads](./leads/index.md)
- [Contacts](./contacts/index.md)
- [Companies](./companies/index.md)
- [Timeline and CRM Activities](./timeline/index.md)
- [CRM Search and Duplicates](./duplicates/index.md)
- [Directories](./status/index.md)
- [Currencies](./currency/index.md)
- [Document Generator](./document-generator/index.md)
- [Digital Workplaces](./automated-solution/index.md)
- [Automation and Triggers](./automation/index.md)
- [Auxiliary Objects](./auxiliary/index.md)
- [Outdated Methods](./outdated/index.md)

The "Universal Methods" section includes a large number of methods that can be used for different CRM entities - deals, leads, contacts, etc.

It is recommended to use universal methods instead of separate methods for deals or leads, for example. Only if universal methods do not allow solving a specific task that can only be addressed by specialized methods for a particular type of entity, should you refer to the specialized sections.

[SPAs](https://helpdesk.bitrix24.com/open/13667300/) are described in the documentation as "[user-defined object types](./universal/user-defined-object-types/index.md)" since they effectively allow the creation of objects that function alongside deals, leads, and other "standard" objects.

{% note alert "" %}

It is not recommended to use the methods described in the "Outdated" section. They continue to function, but their functionality no longer accounts for emerging updates.

{% endnote %}