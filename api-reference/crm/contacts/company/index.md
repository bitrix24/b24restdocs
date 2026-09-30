# Contact-Company Relationship: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A [contact](../index.md) in CRM can be linked to several companies — for example, if the client represents two organizations. Bitrix24 treats one of them as the contact's primary company. An app can add and remove companies one at a time, and read and replace the contact's entire set of companies.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Relationship between deals, contacts, and companies](https://helpdesk.bitrix24.com/open/2519229/)

## Benefits of the Relationship Between Contacts and Companies

The linked companies appear in the contact card and are used by other tools:

- the contact card displays the company data: name, phone number, e-mail, address, company type, and industry
- you can call or send an e-mail from the contact card without navigating to the company card
- when [generating documents from a template](../../document-generator/index.md), symbolic codes insert the data of the linked companies into the document

## How the Relationship Is Structured in the API

The relationship is a separate record, not a contact field, and it has its own fields. For example, this is what the `result` field looks like in the response of the method [crm.contact.company.items.get](./crm-contact-company-items-get.md) for a contact with two companies:

```json
[
    {
        "COMPANY_ID": 7,
        "SORT": 10,
        "ROLE_ID": 0,
        "IS_PRIMARY": "Y"
    },
    {
        "COMPANY_ID": 8,
        "SORT": 20,
        "ROLE_ID": 0,
        "IS_PRIMARY": "N"
    }
]
```

#|
|| **Field** | **What It Means** | **Example Value** ||
|| `COMPANY_ID` | Company identifier. The only required field of the binding. Company identifiers are returned by the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 4` | `7` ||
|| `SORT` | Sorting index: the methods return the contact's companies in this order. If you do not pass a value, Bitrix24 assigns one automatically — the rule differs between [crm.contact.company.add](./crm-contact-company-add.md) and [crm.contact.company.items.set](./crm-contact-company-items-set.md) | `10` ||
|| `IS_PRIMARY` | Whether this is the contact's primary company. Possible values: `Y` — yes, `N` — no | `Y` ||
|| `ROLE_ID` | Reserved field. The link methods do not accept it on write, and new bindings get `0` | `0` ||
|#

Bitrix24 selects the primary company itself and writes it to the contact field `COMPANY_ID`, so there is no need to update this field separately.

#|
|| **What Happened** | **Which Company Becomes Primary** ||
|| The first company was linked to the contact | This company, even if `IS_PRIMARY = N` is passed ||
|| The method [crm.contact.company.add](./crm-contact-company-add.md) added a company with `IS_PRIMARY = Y` | The added company, replacing the previous one ||
|| The method [crm.contact.company.items.set](./crm-contact-company-items-set.md) replaced the set | The first company with `IS_PRIMARY = Y`, or, if there is none, the first company in the list ||
|| The method [crm.contact.company.delete](./crm-contact-company-delete.md) removed the primary company | The first of the remaining companies in ascending order of `SORT` ||
|| All companies were unlinked from the contact | None: the `COMPANY_ID` field is cleared ||
|#

## How to Choose a Method

Some methods of the group work with an individual link, others with the entire set of companies at once.

#|
|| **If You Need To** | **Open the Method** ||
|| Add a single company to the already linked ones | [crm.contact.company.add](./crm-contact-company-add.md) ||
|| Remove a single company and keep the others | [crm.contact.company.delete](./crm-contact-company-delete.md) ||
|| Retrieve the list of the contact's companies | [crm.contact.company.items.get](./crm-contact-company-items-get.md) ||
|| Replace the entire set of companies with the one you pass | [crm.contact.company.items.set](./crm-contact-company-items-set.md) ||
|| Change `SORT` or the primary company among the already linked companies | [crm.contact.company.items.set](./crm-contact-company-items-set.md) — retrieve the set with the method [crm.contact.company.items.get](./crm-contact-company-items-get.md) and pass it in full with the new values. Calling [crm.contact.company.add](./crm-contact-company-add.md) again for an already linked company returns `false` and changes nothing ||
|| Unlink all companies from the contact | [crm.contact.company.items.delete](./crm-contact-company-items-delete.md) ||
|| Retrieve the description of the link fields | [crm.contact.company.fields](./crm-contact-company-fields.md) ||
|#

The mirror task — managing the contacts of a company — is solved by the group of methods [crm.company.contact.*](../../companies/contacts/index.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: a user with the "Edit" access permission for the contact; for [crm.contact.company.items.get](./crm-contact-company-items-get.md), the "Read" access permission is sufficient. The methods [crm.contact.company.add](./crm-contact-company-add.md), [crm.contact.company.delete](./crm-contact-company-delete.md), and [crm.contact.company.items.set](./crm-contact-company-items-set.md) additionally check the "Read" access permission for the companies. The field description [crm.contact.company.fields](./crm-contact-company-fields.md) is available to a user with read access to leads, deals, or other CRM objects, including those in digital workspaces

#| 
|| **Method** | **Description** ||
|| [crm.contact.company.add](./crm-contact-company-add.md) | Adds a company to the specified contact ||
|| [crm.contact.company.delete](./crm-contact-company-delete.md) | Removes a company from the specified contact ||
|| [crm.contact.company.items.get](./crm-contact-company-items-get.md) | Returns a set of companies associated with the specified contact ||
|| [crm.contact.company.items.set](./crm-contact-company-items-set.md) | Establishes a set of companies associated with the specified contact ||
|| [crm.contact.company.items.delete](./crm-contact-company-items-delete.md) | Clears the set of companies associated with the specified contact ||
|| [crm.contact.company.fields](./crm-contact-company-fields.md) | Returns the description of fields for the contact-company relationship ||
|#