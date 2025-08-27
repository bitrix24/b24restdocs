# Find Chats by Names im.search.chat.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.chat.list` performs a search for chats.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **FIND^*^**
[`unknown`](../../data-types.md) | `Mint` | Search phrase | 19 ||
|| **OFFSET**
[`unknown`](../../data-types.md) | `0` | User sample offset | 19 ||
|| **LIMIT**
[`unknown`](../../data-types.md) | `10` | User sample limit | 19 ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

- The search is conducted across the following fields: **Title**, **First Name**, and **Last Name** of chat participants.
- The method supports standard pagination of the Bitrix24 Rest API, but in addition, it allows navigation using the `OFFSET` and `LIMIT` parameters.

## Examples

{% list tabs %}

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'im.search.chat.list',
        {
          FIND: 'Mint'
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative sampling using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('im.search.chat.list', { FIND: 'Mint' }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod with large volumes of data.
    
    try {
      const response = await $b24.callMethod('im.search.chat.list', { FIND: 'Mint' }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.search.chat.list',
                [
                    'FIND' => 'Mint'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'users: ' . print_r($result->data(), true);
        echo 'total: ' . $result->total();
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error searching chat list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.search.chat.list',
        {
            FIND: 'Mint'
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log('users', result.data());
                console.log('total', result.total());
            }
        }
    );
    ```

- PHP CRest

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.search.chat.list',
        Array(
            'FIND' => 'Mint'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

- cURL

    // example for cURL

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Successful Response

```json
{    
    "result": {
        21191: {
            "id": 21191,
            "title": "Mint Chat #3",
            "owner": 2,
            "extranet": false,
            "avatar": "",
            "color": "#4ba984",
            "type": "chat",
            "entity_type": "",
            "entity_data_1": "",
            "entity_data_2": "",
            "entity_data_3": "",
            "date_create": "2017-10-14T12:15:32+02:00",
            "message_type": "C"
        }
    },
    "total": 1
}    
```

### Key Descriptions

- `id` – chat identifier
- `title` – chat title
- `owner` – identifier of the user who owns the chat
- `color` – chat color in hex format
- `avatar` – link to the avatar (if empty, it means the avatar is not set)
- `type` – type of chat (group chat, call chat, open channel chat, etc.)
- `entity_type` – external code for the chat – type
- `entity_id` – external code for the chat – identifier
- `entity_data_1` – external data for the chat
- `entity_data_2` – external data for the chat
- `entity_data_3` – external data for the chat
- `date_create` – chat creation date in ATOM format
- `message_type` – type of chat messages

## Error Response

```json
{
    "error": "FIND_SHORT",
    "error_description": "Too short a search phrase."
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **FIND_SHORT** | Search phrase is too short; the search requires at least three characters. ||
|#