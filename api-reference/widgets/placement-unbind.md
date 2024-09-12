# Removing a Registered Placement Handler

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

{% note info "placement.unbind" %}

**Scope**: [`depending on the placement`](../scopes/permissions.md) | **Who can execute the method**: `administrator`

{% endnote %}

This method removes the registered handler for a placement and returns the number of removed handlers.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **PLACEMENT**^*^ | Identifier of the placement.  | ||
|| **HANDLER** | URL of the placement handler. If the handler URL is not specified, all handlers registered by the application for the specified placement will be removed. | ||
|#

\* - Required parameter.


## Example

```http
GET https://sometestaccount.com/rest/placement.unbind?auth=7e623a5a0000cd710000cd5b00000001000000a8b1dbe022e2de93198634e9526b00f7&placement=CRM_LEAD_DETAIL_TAB&handler=https%3A%2F%2Fwww.myapplicationhost.com%2Fplacement%2F HTTP/1.1
HTTP/1.1 200 OK
{
    "result": {
        "count": 1
    }
}
```

{% include [Footnote on examples](../../_includes/examples.md) %}