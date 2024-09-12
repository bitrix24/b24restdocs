# General Principles for Working with Custom Fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits to meet writing standards

{% endnote %}

{% endif %}

There are several rules for working with **custom fields** for all CRM entities:

1. To correctly invoke methods that work with custom fields, meaning those that include the fragment **userfield** in their names, you should set the corresponding [CRM role](https://helpdesk.bitrix24.com/open/1332384/) with the access permission "Allow to modify settings".

2. To retrieve field names in the current language of the account in methods that work with custom fields, you need to add to the filter:
```
'LANG' => 'en'
```

3. To correctly update multiple fields, such as phone and email, in the CRM, you need to pass the ID of the current value, for example:
```
"PHONE": [ { "ID":245570, "VALUE": "555100501888", "VALUE_TYPE": "WORK" } ],
```

4. To upload files, use the following code:
```
{
    "UF_CRM_1499876148": [
        {"fileData": ["test.txt", "dfgdfgdfgh"]},
        {"fileData": ["test2.txt", "dfgdfgdfgh"]}
    ]
}
```

5. To remove an image from a field, you first need to obtain the ID of the image file in the PHOTO field using the [crm.contact.get](../../contacts/crm-contact-get.md) method, and then pass it with the remove parameter to the [crm.contact.update](../../contacts/crm-contact-update.md) method. For example, in contact 308, we remove the photo with ID 11062 (REGISTER_SONET_EVENT can be omitted):
   
```javascript
BX24.callMethod(
    "crm.contact.update",
    {
        id: 33,
        fields:
        {
            "WEB": {"262014":{"ID":"262014","VALUE":""}}
        },
        params: { "REGISTER_SONET_EVENT": "Y" }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
        {
            console.info(result.data());
        }
    }
);
```
{% include [Footnote about examples](../../../../_includes/examples.md) %}