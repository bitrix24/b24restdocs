# Retrieving a List of Available Placement Options for the Application

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- revisions needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response not provided
- error response not provided
- links to pages not yet created are missing.

{% endnote %}

{% endif %}

{% note info "placement.list" %}

**Scope**: [`depending on the placement`](../scopes/permissions.md) | **Who can execute the method**: `for everyone`

{% endnote %}

This method retrieves a list of available placement options for the application.

## Parameters

#|
|| **Parameter** | **Description** | **Available Since** ||
|| **SCOPE** | Restricts the list to one of the application's access permissions | ||
|#

## Example

```http
GET https://sometestaccount.bitrix24.com/rest/placement.list?auth=7e623a5a0000cd710000cd5b00000001000000a8b1dbe022e2de93198634e9526b00f7 HTTP/1.1
HTTP/1.1 200 OK
{
    "result": [
        "CRM_LEAD_LIST_MENU",
        "CRM_DEAL_LIST_MENU",
        "CRM_INVOICE_LIST_MENU",
        "CRM_QUOTE_LIST_MENU",
        "CRM_CONTACT_LIST_MENU",
        "CRM_COMPANY_LIST_MENU",
        "CRM_ACTIVITY_LIST_MENU",
        "CRM_LEAD_DETAIL_TAB",
        "CRM_DEAL_DETAIL_TAB",
        "CRM_CONTACT_DETAIL_TAB",
        "CRM_COMPANY_DETAIL_TAB",
        "CRM_LEAD_DETAIL_ACTIVITY",
        "CRM_DEAL_DETAIL_ACTIVITY",
        "CRM_CONTACT_DETAIL_ACTIVITY",
        "CRM_COMPANY_DETAIL_ACTIVITY"
    ]
}
```

{% include [Footnote on examples](../../_includes/examples.md) %}