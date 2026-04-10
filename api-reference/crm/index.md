# Overview of CRM

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

[CRM](https://helpdesk.bitrix24.com/open/25766191/) is one of the most functional tools in Bitrix. To effectively use the REST API, it is highly recommended to familiarize yourself with the product's features and key user scenarios. Understanding the relationships between CRM entities will simplify the search for and use of the REST API tools needed to implement the desired functionality.

The current section of the documentation includes the following main sections:

- [Sales Intelligence](./tracking/index.md)
- [Universal Methods](./universal/index.md)
- [Deals](./deals/index.md)
- [Leads](./leads/index.md)
- [Contacts](./contacts/index.md)
- [Companies](./companies/index.md)
- [Timeline and CRM activities](./timeline/index.md)
- [Searching and Processing Duplicates in CRM](./duplicates/index.md)
- [Directories](./status/index.md)
- [Currencies](./currency/index.md)
- [Document Generator](./document-generator/index.md)
- [Digital Workplaces](./automated-solution/index.md)
- [Automation and Triggers](./automation/index.md)
- [Auxiliary Objects](./auxiliary/index.md)
- [Outdated Methods](./outdated/index.md)

The "Universal Methods" section includes a large number of methods that can be used for various CRM entities — deals, leads, contacts, etc.

It is recommended to use universal methods instead of separate methods for deals or leads, for example. Only if universal methods do not allow you to solve a specific task that can only be addressed by specialized methods for particular types of entities should you refer to the specialized sections.

[Smart processes](https://helpdesk.bitrix24.com/open/19141012/) are described in the documentation as "[user-defined types of CRM objects](./universal/user-defined-object-types/index.md)" since they effectively allow the creation of objects that function alongside deals, leads, and other "standard" entities.

{% note alert "" %}

It is not recommended to use the methods described in the "Outdated" section. They continue to function, but their capabilities do not account for emerging updates.

{% endnote %}