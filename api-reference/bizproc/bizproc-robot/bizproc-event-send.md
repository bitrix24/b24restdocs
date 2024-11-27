# Automation rules and workflow actions with result waiting bizproc.event.send

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

This article explains how result waiting works for Automation rules and workflow actions. Key points:

- In which scenarios are such Automation rules and actions needed?
- What are the drawbacks and features?
- How does it work? Here, the method bizproc.event.send comes into play in its standard format.

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the output parameters of the action specified in the action description.

#|
|| Parameter     | Description  ||
|| **EVENT_TOKEN** | A unique key that must be used when sending an event to the workflow. The value of this token is obtained by the handler of the Automation rule or workflow action in the array of input data passed.    ||
|| **RETURN_VALUES** | An array of returned values from the action. It will also appear in the _Insert value_ form under the _Additional results_ tab. ||
|#

## Example

```javascript
var params = {
    event_token: '55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90',
    return_values: {
        outputString: '846c55d14f552180874a628d2615e285'
    }
};

BX24.callMethod(
    'bizproc.event.send',
    params,
    function(result) {
        if(result.error())
            alert("Error: " + result.error());
        else
            alert("Success: " + result.data());
    }
);
```