# Process Files

The REST service methods receive files as a string encoded in `base64`. You can also send a regular array, where the first element is the file name and the second is the content in `base64`.

In the case of a fully client-side application, you can either use the [FileReader](http://www.w3.org/TR/FileAPI/) object or simply provide a reference to the file input element (`<input type="file">`) as the value of the request field.

## Example

```html
<input type="file" id="testfile"><br />
<span onclick="sendInputFile()">send file from inputsend static file</span>
<script>
function sendInputFile() {
    BX24.init(() => {
    
        BX24.callMethod('entity.item.add', {
            'ENTITY': 'menu',
            'NAME': Math.random(),
            'DETAIL_PICTURE': document.getElementById('testfile')
        }, function(){
            alert('Finished!');
        });
    });
}
/*
POST https://my.bitrix24.com/rest/entity.item.add.json HTTP/1.1
Host: my.bitrix24.com
Content-Length: 186
Content-Type: text/plain; charset=UTF-8
auth=6a8c365cb010ba42bd5b0f6ae803f47c&ENTITY=menu&NAME=0.2630483947652045&DETAIL_PICTURE[0]=1.gif&DETAIL_PICTURE[1]=R0lGODlhAQABAIAAAP%2F%2F%2FwAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw%3D%3D
*/
function sendStaticFile()  {
    BX24.init(() => {
    
        BX24.callMethod('entity.item.add', {
            'ENTITY': 'menu',
            'NAME': '1.gif',
            'DETAIL_PICTURE': ['1.gif', 'R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==']
        }, function(){
            alert('Finished!');
        });
    });
}
/*
POST https://my.bitrix24.com/rest/entity.item.add.json HTTP/1.1
Host: my.bitrix24.com
Content-Length: 173
Content-Type: text/plain; charset=UTF-8
auth=6a8c365cb010ba42bd5b0f6ae803f47c&ENTITY=menu&NAME=1.gif&DETAIL_PICTURE[0]=1.gif&DETAIL_PICTURE[1]=R0lGODlhAQABAIAAAP%2F%2F%2FwAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw%3D%3D
*/
</script>
```

{% include [Footnote on examples](../../../_includes/examples.md) %}

For the [CRM](../../crm/index.md) methods, when adding an image for a product, instead of

```php
'DETAIL_PICTURE': ['1.gif', 'R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==']
```

use

```php
"PREVIEW_PICTURE": {"fileData": ["1.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]}
```

This feature is due to the fact that CRM supports file deletion.

To remove an image from the field, you first need to get the `ID` of the image file in the `PHOTO` field using the [crm.contact.get](../../crm/contacts/crm-contact-get.md) method and then pass it with the `remove` parameter to the [crm.contact.update](../../crm/contacts/crm-contact-update.md) method. For example, in contact 308, remove the photo with ID 11062 (`REGISTER_SONET_EVENT` can be omitted):

```js
BX24.init(() => {
    BX24.callMethod(
        "crm.contact.update",
        {
            id: 308,
            fields:
            {
                "PHOTO": {id: 11062, remove: 'Y'}
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
});
```

## Continue Learning

- [{#T}](./bx24-call-bind.md)
- [{#T}](./bx24-call-unbind.md)
- [{#T}](./bx24-call-method.md)
- [{#T}](./bx24-call-batch.md)