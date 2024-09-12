# EntityId Identifiers

The association of a field with a specific entity is determined by the value of the **entityId** field. Below are the identifiers for different entities:

## CRM

- Lead – `CRM_LEAD`
- Contact – `CRM_CONTACT`
- Company – `CRM_COMPANY`
- Deal – `CRM_DEAL`
- Estimate – `CRM_QUOTE`
- Invoice – `CRM_INVOICE`
- SPA – `CRM_ + {ID}`, where ID is the identifier of the SPA (not the type identifier)
- New invoices – `CRM_SMART_INVOICE`

## RPA

In the RPA module, the field is constructed using the rule `RPA_ + {ID}`, where ID is the identifier of the process.