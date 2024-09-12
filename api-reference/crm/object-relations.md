# Structure of CRM Object Relationships

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- This section needs very thoughtful material to help understand the complexities of CRM.
- The first section is about basic objects. We discuss the relationships between them, related to the conversion from lead, as well as the relationship between them and deals one-to-many. It is noted that this is just a basic binding of deals; in reality, it is more complex and will be discussed below.
- The second section is about the relationships of deals and objects using the BINDINGS many-to-many table.
- The third section is about attributes. First, a general overview, then a more detailed scheme using a company as an example.

{% endnote %}

{% endif %}

## Basic CRM Objects

```mermaid
%%{init: { "theme": "forest" } }%%
erDiagram
    LEAD |o--|| DEAL : "DEAL.LEAD_ID - LEAD.ID" 
    LEAD |o--|| COMPANY : "COMPANY.LEAD_ID - LEAD.ID" 
    LEAD |o--|| CONTACT : "CONTACT.LEAD_ID - LEAD.ID" 
    LEAD |o--|| QUOTE : "QUOTE.LEAD_ID - LEAD.ID" 
    LEAD {
        integer ID
        string TITLE
    }
    DEAL {
        integer ID
        string TITLE
        integer LEAD_ID
    }
    CONTACT {
        integer ID
        string NAME
        string LAST_NAME
        integer LEAD_ID
    }
    COMPANY {
        integer ID
        string TITLE
        integer LEAD_ID
    }
    QUOTE {
        integer ID
        string TITLE
        integer LEAD_ID
    }
    CRM_SMART_INVOICE {
        integer ID
        string TITLE
        integer LEAD_ID
    }
    ACTIVITY ||--|| DEAL : "ACTIVITY.OWNER_ID - DEAL.ID" 
    ACTIVITY ||--|| COMPANY : "ACTIVITY.OWNER_ID - COMPANY.ID" 
    ACTIVITY ||--|| CONTACT : "ACTIVITY.OWNER_ID - CONTACT.ID" 
    ACTIVITY ||--|| LEAD : "ACTIVITY.OWNER_ID - LEAD.ID" 
    ACTIVITY ||--|| CRM_SMART_INVOICE : "ACTIVITY.OWNER_ID - CRM_SMART_INVOICE.ID" 
    ACTIVITY {
        integer ID
        integer OWNER_ID
        integer OWNER_TYPE_ID
        integer TYPE_ID
    }
```

## Multiple Relationships with Deals

```mermaid
%%{init: { "theme": "forest" } }%%
erDiagram
    LEAD {
        integer ID
        string TITLE
    }
    DEAL {
        integer ID
        string TITLE
    }
    CONTACT {
        integer ID
        string NAME
        string LAST_NAME
    }
    COMPANY {
        integer ID
        string TITLE
    }
    CRM_SMART_INVOICE {
        integer ID
        string TITLE
    }
    BINDINGS }|--|| DEAL : "BINDINGS.ENTITY_ID - DEAL.ID" 
    BINDINGS }|--|| COMPANY : "BINDINGS.ENTITY_ID - COMPANY.ID" 
    BINDINGS }|--|| CONTACT : "BINDINGS.ENTITY_ID - CONTACT.ID" 
    BINDINGS }|--|| LEAD : "BINDINGS.ENTITY_ID - LEAD.ID" 
    BINDINGS }|--|| CRM_SMART_INVOICE : "BINDINGS.ENTITY_ID - LEAD.ID" 
    BINDINGS }|--|| ACTIVITY : "BINDINGS.ENTITY_ID - ACTIVITY.ID" 
    BINDINGS {
        integer ACTIVITY_ID
        integer ENTITY_ID
        integer ENTITY_TYPE_ID
    }
    ACTIVITY {
        integer ID
        integer OWNER_ID
        integer OWNER_TYPE_ID
        integer TYPE_ID
    }
```