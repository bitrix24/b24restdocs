# Overview of CRM

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

[CRM](https://helpdesk.bitrix24.com/open/21793278/) is one of the most functional tools in Bitrix. To effectively use the REST API, it is highly recommended to study the product's functionality and the main user scenarios. Understanding the relationships between CRM objects will simplify the search and use of the REST API tools needed to implement the required functionality.

The current section of the documentation includes the main sections:

- [Universal methods](./universal/index.md)
- [Deals](./deals/index.md)
- [Leads](./leads/index.md)
- [Contacts](./contacts/index.md)
- [Companies](./companies/index.md)
- [Timeline and activities](./timeline/index.md)
- [Searching and processing duplicates in CRM](./duplicates/index.md)
- [Reference books](./status/index.md)
- [Currencies](./currency/index.md)
- [Document generator](./document-generator/index.md)
- [Digital workplaces](./automated-solution/index.md)
- [Automation and triggers](./automation/index.md)
- [Auxiliary objects](./auxiliary/index.md)
- [Deprecated methods](./outdated/index.md)

The "Universal methods" section includes a large number of methods that can be used for different CRM objects — deals, leads, contacts, etc.

It is recommended to use universal methods instead of separate methods for deals or leads, for example. Only if universal methods do not allow solving a task that can only be addressed by specialized methods for specific types of objects should one refer to the specialized sections.

[Smart processes](https://helpdesk.bitrix24.com/open/13667300/) in the documentation are described as "[user-defined object types](./universal/user-defined-object-types/index.md)" since they essentially allow the creation of objects that function alongside deals, leads, and other "standard" objects.

{% note alert "" %}

It is not recommended to use the methods described in the "Deprecated" section. They continue to function, but their functionality no longer accounts for emerging updates.

{% endnote %}