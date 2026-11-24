# How to Find a CRM Object Created from an Open Channel Conversation

> Scope: [`imopenlines`](../../api-reference/scopes/permissions.md), [`crm`](../../api-reference/scopes/permissions.md)
>
> Who can execute the scenario: an application user who participates in the Open Channel chat or can join conversations of this channel, and has permission to read the required CRM items
> - [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) — any user, called only with application authorization
> - [imopenlines.dialog.get](../../api-reference/imopenlines/openlines/sessions/imopenlines-dialog-get.md) — a participant of the Open Channel chat or a user who can join conversations of this channel
> - [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) — a user with permission to read CRM items

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The Open Channel connector passes customer messages from an external channel—such as a messenger, a website chat, or a helpdesk service—to Bitrix24. If the CRM tracker creates a lead or a deal with a contact based on an inquiry, or finds an existing contact or company, the integration often needs to retrieve the identifier of the linked CRM object.

Do not pass technical identifiers into the `email` field of the [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) method. The CRM tracker saves a correctly formatted address to the customer card and uses it to search for existing contacts and companies. A marker formatted as an address ends up in the card, and inquiries from different customers with a matching marker can be linked to the same contact.

## How to Retrieve the Identifier of a Linked CRM Object

To find the lead, contact, or company that the CRM tracker linked to an Open Channel conversation, execute the following methods in sequence:

1. [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) — send a customer message to an Open Channel and retrieve the chat identifier

2. [imopenlines.dialog.get](../../api-reference/imopenlines/openlines/sessions/imopenlines-dialog-get.md) — retrieve the conversation data and the Open Channel user code

3. [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) — find the lead, contact, or company using the exact value of the messenger field

## Requirements

- An application with the `imopenlines` and `crm` permissions. The `imconnector.send.messages` method works only in the application context, an inbound webhook is not suitable for it
- An Open Channel with CRM integration enabled: its settings specify which CRM object to create from an inquiry
- A connector registered and connected to the Open Channel. How to do this is described in the tutorial [{#T}](./example-connector.md)
- A connector code from the `ID` parameter of the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method. The examples use `myconnector`
- An Open Channel identifier. The examples use `107`
- An external chat or channel identifier. The examples use `channel-123`
- An external customer identifier. The examples use `ext-user-42`

Run the JS examples on an application page where the `BX24` library is loaded. Run the PHP examples on the application server with the [CRest](../../sdk/crest-php-sdk/index.md) library configured. Replace the connector, line, customer, chat, and message identifiers, the phone number, the message text, and the chat URL in the examples with the data of your integration. The step 1 example sends a real message to the Open Channel, so test the scenario on a test channel.

## 1. Send a Customer Message

Send a message to an Open Channel using the [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) method. Specify the customer's contact details in the `user` block.

- `CONNECTOR` — the connector code passed in the `ID` parameter of the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method. We will use `myconnector`

- `LINE` — the Open Channel identifier. We will use `107`

- `MESSAGES` — an array of messages. Pass the customer data, messages, and the external chat information

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

   ```js
   BX24.callMethod(
       'imconnector.send.messages',
       {
           CONNECTOR: 'myconnector',
           LINE: 107,
           MESSAGES: [
               {
                   user: {
                       id: 'ext-user-42',
                       name: 'Klaus',
                       phone: '+499990000000'
                   },
                   message: {
                       id: 'ext-msg-1001',
                       date: Math.floor(Date.now() / 1000),
                       text: 'Good day'
                   },
                   chat: {
                       id: 'channel-123',
                       name: 'Support channel',
                       url: 'https://example.com/chats/123'
                   }
               }
           ]
       },
       function(result)
       {
           if(result.error())
           {
               console.error(result.error());
           }
           else
           {
               console.dir(result.data());
           }
       }
   );
   ```

- PHP

   ```php
   require_once('crest.php');

   $result = CRest::call(
       'imconnector.send.messages',
       [
           'CONNECTOR' => 'myconnector',
           'LINE' => 107,
           'MESSAGES' => [
               [
                   'user' => [
                       'id' => 'ext-user-42',
                       'name' => 'Klaus',
                       'phone' => '+499990000000',
                   ],
                   'message' => [
                       'id' => 'ext-msg-1001',
                       'date' => time(),
                       'text' => 'Good day',
                   ],
                   'chat' => [
                       'id' => 'channel-123',
                       'name' => 'Support channel',
                       'url' => 'https://example.com/chats/123',
                   ],
               ],
           ],
       ]
   );

   echo '<pre>' . htmlspecialchars(print_r($result, true), ENT_QUOTES, 'UTF-8') . '</pre>';
   ```

{% endlist %}

If the message is sent successfully, the method returns the Open Channel session data in the `session` block.

```json
{
    "result": {
        "SUCCESS": true,
        "DATA": {
            "RESULT": [
                {
                    "SUCCESS": true,
                    "session": {
                        "ID": "323",
                        "CHAT_ID": "1767"
                    }
                }
            ]
        }
    }
}
```

If the message is accepted, its result contains `SUCCESS: true` and the `session` block: `result.DATA.RESULT[0].session` for the first message in `MESSAGES`. If `SUCCESS` is `false`, the reasons are listed in the `ERRORS` array of this message. If `SUCCESS` is `false` or the `session` block is missing, do not proceed to step 2: check the message processing result and the connector connection to the line. Save the following values:

- `session.ID` — the Open Channel session identifier

- `session.CHAT_ID` — the Bitrix24 chat identifier. This will be required in the next step

## 2. Retrieve Dialog Data

Retrieve the chat data using the [imopenlines.dialog.get](../../api-reference/imopenlines/openlines/sessions/imopenlines-dialog-get.md) method. Pass the value of `session.CHAT_ID` from the `imconnector.send.messages` method response to the `CHAT_ID` parameter. The step examples run separately: replace `1767` with your `session.CHAT_ID` value.

{% list tabs %}

- JS

   ```js
   BX24.callMethod(
       'imopenlines.dialog.get',
       {
           CHAT_ID: 1767
       },
       function(result)
       {
           if(result.error())
           {
               console.error(result.error());
           }
           else
           {
               const dialog = result.data();
               // Messenger field value for step 3
               const imolValue = 'imol|' + dialog.entity_id;
               console.log(imolValue);
               // CRM links the dialog already has
               console.log(dialog.entity_data_2);
           }
       }
   );
   ```

- PHP

   ```php
   require_once('crest.php');

   $result = CRest::call(
       'imopenlines.dialog.get',
       [
           'CHAT_ID' => 1767,
       ]
   );

   if (isset($result['error'])) {
       echo htmlspecialchars($result['error_description'], ENT_QUOTES, 'UTF-8');
   } else {
       // Messenger field value for step 3
       $imolValue = 'imol|' . $result['result']['entity_id'];
       echo htmlspecialchars($imolValue, ENT_QUOTES, 'UTF-8') . '<br>';
       // CRM links the dialog already has
       echo htmlspecialchars($result['result']['entity_data_2'], ENT_QUOTES, 'UTF-8');
   }
   ```

{% endlist %}

Two fields in the response are important:

- `entity_id` — the Open Channel user code in the `<connector code>|<line ID>|<external chat ID>|<user ID>` format. The last part is the ID of the user Bitrix24 created for the customer, not the external identifier from `user.id`. The value of the Messenger field in the CRM is built from `entity_id`

- `entity_data_2` — a string containing the CRM links already associated with the dialog: "type|identifier" pairs for the lead, company, contact, and deal, where `0` means there is no link. Take the deal only from here: a deal has no messenger field, and step 3 will not find it

```json
{
    "result": {
        "id": 1767,
        "entity_type": "LINES",
        "entity_id": "myconnector|107|channel-123|ext-user-42",
        "entity_data_2": "LEAD|0|COMPANY|0|CONTACT|42|DEAL|0",
        "dialog_id": "chat1767"
    }
}
```

If `entity_data_2` contains a non-zero identifier for the required type, use it as the current dialog link: in the example, contact `42`. Step 3 finds objects with the same messenger value. You need it when there is no link or when you need to find an object of a different type. To make sure the found object is linked to this dialog, compare its identifier with `entity_data_2`.

To obtain the value for the Messenger field in the CRM, add the `imol|` prefix to `entity_id`, as in the code examples above. In the example, this is:

```text
imol|myconnector|107|channel-123|ext-user-42
```

## 3. Find a CRM Object by the Messenger Field

Perform an exact search by the messenger field using the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method. Pass the CRM object type in the `entityTypeId` parameter:

- `1` — lead
- `3` — contact
- `4` — company

In universal CRM methods, the Open Channel messenger value is available in the `imol` field. All multi-fields of an object — phone numbers, email addresses, messengers — are returned in the `fm` field if you specify it in `select`.

For a lead, contact, and company, the `imol` field contains only the first Open Channel messenger value. If the value of the current dialog differs from the first saved value, the search will not find this object. So check the link in `entity_data_2` first, and do not treat an empty search result as proof that the object does not exist.

In the example, we will find a contact, so we will pass `entityTypeId = 3`. Replace the example `imolValue` with the value retrieved in step 2.

- `entityTypeId` — the CRM object type. For a contact, specify `3`

- `filter[=imol]` — the `imolValue` value from step 2: the `imol|` prefix and `entity_id` from the `imopenlines.dialog.get` method response

- `select` — the list of fields to return. To search for an identifier, the `id` field is sufficient; the example adds `imol` for verification

{% list tabs %}

- JS

   ```js
   // imolValue — the value from step 2
   const imolValue = 'imol|myconnector|107|channel-123|1571';

   BX24.callMethod(
       'crm.item.list',
       {
           entityTypeId: 3,
           select: [
               'id',
               'imol'
           ],
           filter: {
               '=imol': imolValue
           }
       },
       function(result)
       {
           if(result.error())
           {
               console.error(result.error());
           }
           else
           {
               console.dir(result.data());
           }
       }
   );
   ```

- PHP

   ```php
   require_once('crest.php');

   // $imolValue — the value from step 2
   $imolValue = 'imol|myconnector|107|channel-123|1571';

   $result = CRest::call(
       'crm.item.list',
       [
           'entityTypeId' => 3,
           'select' => [
               'id',
               'imol',
           ],
           'filter' => [
               '=imol' => $imolValue,
           ],
       ]
   );

   echo '<pre>' . htmlspecialchars(print_r($result, true), ENT_QUOTES, 'UTF-8') . '</pre>';
   ```

{% endlist %}

If the contact is found, the method returns its data in the `items` array.

```json
{
    "result": {
        "items": [
            {
                "id": 42,
                "imol": "imol|myconnector|107|channel-123|1571"
            }
        ]
    },
    "total": 1
}
```

The value of `items[0].id` is the identifier of the found CRM object. In the example, a contact with identifier `42` was found.

If `items` is an empty array, no contact with this first messenger value was found. For a lead and a company, repeat the request with `entityTypeId = 1` and `entityTypeId = 4`.

## Verify the Result

Open the CRM card with the `items[0].id` identifier and check that the Messenger field contains the Open Channel. The current dialog link to the object is confirmed by the object's identifier in `entity_data_2` from step 2.

## If the CRM Object Is Not Found

Check the values that link the scenario steps:

- the `imconnector.send.messages` response contains the `session` block with `CHAT_ID`
- `CHAT_ID` from the `imconnector.send.messages` response is passed to the `imopenlines.dialog.get` request
- the `imopenlines.dialog.get` response contains a non-empty `entity_id` field
- a value with the `imol|` prefix is passed to the `crm.item.list` filter, for example `imol|myconnector|107|channel-123|1571`
- `entityTypeId` specifies the CRM object type that the CRM tracker might have created or linked to the dialog: `1`, `3`, or `4`
- CRM integration is enabled in the Open Channel settings, and the required type of object to create is set. After changing the settings, test the scenario on a new inquiry
- the user on whose behalf `crm.item.list` is performed has permission to read the found CRM object

If a method returns an error, check the request data.

- `NOT_ACTIVE_LINE` from `imconnector.send.messages` — the line with this `LINE` is inactive or the connector is not connected to it. Check the line identifier and the connector connection, then repeat step 1
- `WRONG_AUTH_TYPE` from `imconnector.send.messages` — the request was not made in the application context. Call the method with application authorization and repeat step 1
- `ACCESS_ERROR` from `imopenlines.dialog.get` — a chat with this `CHAT_ID` is not found or the user has no access to the dialog. Check `CHAT_ID` from step 1 and repeat step 2
- `INVALID_ARG_VALUE` from `crm.item.list` — an invalid request parameter, for example, the filter contains a field that cannot be used for filtering. The error text names the field. The `=imol` filter works only for a lead, contact, and company: with `entityTypeId = 2`, the method returns this error. Check the field and `entityTypeId`, then repeat step 3

## Key Considerations

- Pass only the customer's real address in `email`: the CRM tracker uses it to search for existing contacts and companies
- Check several CRM object types: the CRM tracker can create a lead or a contact, or link the dialog to an existing company
