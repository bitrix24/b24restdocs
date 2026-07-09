# How to Find a CRM Object Created from an Open Channel Conversation

> Scope: [`imopenlines`](../../api-reference/scopes/permissions.md), [`crm`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods:
> - `imconnector.send.messages` — an application with access to Open Channels
> - `imopenlines.dialog.get` — a user with permission to access the conversation
> - `crm.item.list` — a user with permission to read CRM items

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Open Channel connector passes customer messages from an external channel—such as a messenger, a website chat, or a helpdesk service—to Bitrix24. If the CRM tracker creates a lead, contact, or company based on an inquiry, the integration often needs to retrieve the identifier of the created CRM object.

Do not pass technical identifiers into the `email` field of the [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) method. In the CRM, this field is used as the customer's email address. An invalid value could be saved to a contact or company and interfere with scenarios requiring a valid email, such as sending a receipt to a customer.

## How to Retrieve the Identifier of a Linked CRM Object

To find the lead, contact, or company that the CRM tracker linked to an Open Channel conversation, execute the following methods in sequence:

1. [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) — send a customer message to an Open Channel and retrieve the chat identifier

2. [imopenlines.dialog.get](../../api-reference/imopenlines/openlines/sessions/imopenlines-dialog-get.md) — retrieve the conversation data and the Open Channel user code

3. [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) — find the lead, contact, or company using the exact value of the messenger field

## Requirements

- An application with access to Open Channels and CRM
- A connector code from the `ID` parameter of the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method. The examples use `myconnector`
- An Open Channel identifier. The examples use `107`
- An external chat or channel identifier. The examples use `channel-123`
- An external customer identifier. The examples use `ext-user-42`

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

   echo '<PRE>';
   print_r($result);
   echo '</PRE>';
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

Save the following values:

- `session.ID` — the Open Channel session identifier

- `session.CHAT_ID` — the Bitrix24 chat identifier. This will be required in the next step

## 2. Retrieve Dialog Data

Retrieve the chat data using the [imopenlines.dialog.get](../../api-reference/imopenlines/openlines/sessions/imopenlines-dialog-get.md) method. Pass the value of `session.CHAT_ID` from the `imconnector.send.messages` method response to the `CHAT_ID` parameter.

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
               console.dir(result.data());
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

   echo '<PRE>';
   print_r($result);
   echo '</PRE>';
   ```

{% endlist %}

Two fields in the response are important:

- `entity_id` — the Open Channel user code. This can be used to construct the value for the Messenger field in the CRM

- `entity_data_2` — a string containing the CRM links already associated with the dialog

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

To obtain the value for the Messenger field in the CRM, add the `imol|` prefix to `entity_id`. In the example, this is:

```text
imol|myconnector|107|channel-123|ext-user-42
```

## 3. Find a CRM Object by the Messenger Field

Perform an exact search by the messenger field using the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method. Pass the CRM object type in the `entityTypeId` parameter:

- `1` — lead
- `3` — contact
- `4` — company

In universal CRM methods, the IMOL messenger value is available in the `imol` field. Field `fm` with multi-fields cannot be explicitly requested via `select`. To receive `fm` in the response, pass `select: ['*']`.

In the example, we will find a contact, so we will pass `entityTypeId = 3`.

- `entityTypeId` — the CRM object type. For a contact, specify `3`

- `filter[=imol]` — the Messenger field value: the `imol|` prefix and `entity_id` from the `imopenlines.dialog.get` method response

- `select` — the list of fields to return. To search for an identifier, the `id` field is sufficient

{% list tabs %}

- JS

   ```js
   BX24.callMethod(
       'crm.item.list',
       {
           entityTypeId: 3,
           select: [
               'id',
               'title',
               'imol'
           ],
           filter: {
               '=imol': 'imol|myconnector|107|channel-123|ext-user-42'
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

   $result = CRest::call(
       'crm.item.list',
       [
           'entityTypeId' => 3,
           'select' => [
               'id',
               'title',
               'imol',
           ],
           'filter' => [
               '=imol' => 'imol|myconnector|107|channel-123|ext-user-42',
           ],
       ]
   );

   echo '<PRE>';
   print_r($result);
   echo '</PRE>';
   ```

{% endlist %}

If the contact is found, the method returns its data in the `items` array.

```json
{
    "result": {
        "items": [
            {
                "id": 42,
                "title": "Klaus",
                "imol": "imol|myconnector|107|channel-123|ext-user-42"
            }
        ]
    },
    "total": 1
}
```

The value of `items[0].id` is the identifier of the found CRM object. In the example, a contact with identifier `42` was found.

If the contact is not found, repeat the request with `entityTypeId = 1` for a lead and `entityTypeId = 4` for a company.

## If the CRM Object Is Not Found

Check the values that link the scenario steps:

- the `imconnector.send.messages` response contains the `session` block with `CHAT_ID`
- `CHAT_ID` from the `imconnector.send.messages` response is passed to the `imopenlines.dialog.get` request
- the `imopenlines.dialog.get` response contains a non-empty `entity_id` field
- a value with the `imol|` prefix is passed to the `crm.item.list` filter, for example `imol|myconnector|107|channel-123|ext-user-42`
- `entityTypeId` specifies the CRM object type that the CRM tracker might have created: `1`, `3`, or `4`
- the user on whose behalf `crm.item.list` is performed has permission to read the found CRM object

If the `crm.item.list` method returns error `INVALID_ARG_VALUE`, check the field in the filter. To search by messenger use `=imol`.

## Key Considerations

- Do not use the `email` field for technical markers. Pass only the actual customer email address to it
- Retain `session.CHAT_ID` and `session.ID` from the [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) response
- Use `entity_id` from [imopenlines.dialog.get](../../api-reference/imopenlines/openlines/sessions/imopenlines-dialog-get.md) with the `imol|` prefix as the exact value for searching by messenger
- Check multiple CRM object types if the CRM tracker configurations can create leads, contacts, or companies
