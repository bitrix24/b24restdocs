# Access Permissions for Methods

Access permissions for executing specific REST API methods are managed through a scope mechanism (SCOPE). When you add a mass-market solution in the Partner's area or an on-premise solution to your specific Bitrix24, you specify the list of necessary Bitrix24 scopes required for the application to function.

The binding to a specific scope is indicated in the description of each REST method at the very beginning. For example,

> Scope: `CRM`
>
> Who can execute the method: any user

Also, pay attention to the note "Who can execute the method." Some methods can only be called on behalf of a user with administrative rights in a specific Bitrix24.

Let's consider a specific situation where your solution integrates Bitrix24 with external telephony, and you are using the methods `telephony.externalcall.register` and `telephony.externalcall.finish`, which also add leads to the CRM, but do not explicitly call CRM methods like `crm.lead.add` and `crm.activity.add`. In this case, your application will require the telephony scope, while the crm scope will not be necessary.

- [{#T}](./permissions.md)
- [{#T}](./confirmation.md)