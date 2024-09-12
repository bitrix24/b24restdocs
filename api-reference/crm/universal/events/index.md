# Overview of CRM Object Events

There are three types of events — for adding, updating, and deleting a SPA element.

Events are triggered by actions on elements of ALL SPAs. Filtering by the desired type will need to be done on the event processing side.

The list of available events will include the following set:

- **onCrmDynamicItemAdd** — adding an element of any SPA
- **onCrmDynamicItemUpdate** — updating an element of any SPA
- **onCrmDynamicItemDelete** — deleting an element of any SPA

There is a theoretical possibility to subscribe to events of elements of a specific SPA. The names of such events look like `onCrmDynamicItemAdd_{entityTypeId}`. They are not displayed in the webhook creation interface but may work when subscribed through applications.

The events `onCrmDynamicItemAdd`, `onCrmDynamicItemUpdate`, and `onCrmDynamicItemDelete` should only be used for [User-defined CRM Types](../user-defined-object-types/index.md) and Invoices, as these events are not triggered for system CRM types.

More details on which events can be used for Leads, Deals, and so on are described on the event detail page.

## Events

- [{#T}](on-crm-dynamic-item-add.md)
- [{#T}](on-crm-dynamic-item-update.md)
- [{#T}](on-crm-dynamic-item-delete.md)