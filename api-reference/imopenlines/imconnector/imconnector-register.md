# Register the connector imconnector.register

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- - these files were not in the structure, but there are links to them in the Note: imconnector.connector.data.set and cases/example_connector_chat

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method registers a new type of connector.

{% note info "Note" %}

If you want the connector to appear in the general list of connectors in the widget on the website, you need to use the method `imconnector.connector.data.set`. 

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | A unique identifier for the connector. It is better to prefix it with your own prefix to avoid conflicts with current and potential future connector IDs. You can use: numbers, letters, underscore. | ||
|| **NAME^*^**
[`unknown`](../../data-types.md) | The display name of the connector. | ||
|| **ICON^*^**
[`unknown`](../../data-types.md) | An array describing the connector's icon, where:
- **DATA_IMAGE^*^**
[`unknown`](../../data-types.md) - DATA representation of the SVG icon. [Example](*key_example)
- **COLOR** - color. Example: `#1900ff`
- **SIZE** - size. Example: `90%`
- **POSITION** - position of the SVG. Example: `center`
 | ||
|| **PLACEMENT_HANDLER^*^**
[`unknown`](../../data-types.md) | A link to the URL of the embedding handler that will be called to show users the connector's settings interface in the slider. [More details](https://training.bitrix24.com/support/training/course/index.php?COURSE_ID=169&CHAPTER_ID=020068&LESSON_PATH=13643.20052.20068). | ||
|| **ICON_DISABLED**
[`unknown`](../../data-types.md) | An array describing the connector's icon for the **inactive** variant, where:
- **DATA_IMAGE^*^** - DATA representation of the SVG icon. [Example](*key_example)
- **COLOR** - color. Example: `#1900ff`
- **SIZE** - size. Example: `90%`
- **POSITION** - position of the SVG. Example: `center`
  | ||
|| **DEL_EXTERNAL_MESSAGES**
[`unknown`](../../data-types.md) | Is it possible to delete incoming messages? Default: yes. | ||
|| **EDIT_INTERNAL_MESSAGES**
[`unknown`](../../data-types.md) | Is it possible to edit your own messages? Default: yes. | ||
|| **DEL_INTERNAL_MESSAGES**
[`unknown`](../../data-types.md) | Is it possible to delete your own messages? Default: yes. | ||
|| **NEWSLETTER**
[`unknown`](../../data-types.md) | Is it possible to use the channel for CRM mailing? Default: yes. | ||
|| **NEED_SYSTEM_MESSAGES**
[`unknown`](../../data-types.md) | Is it possible to send system messages to the channel? Examples: greetings, closing remarks, etc. Default: yes. | ||
|| **NEED_SIGNATURE**
[`unknown`](../../data-types.md) | Is it possible to send a signature in the message itself? Example: a line with the operator's name is added before the message text. Default: yes. | ||
|| **CHAT_GROUP**
[`unknown`](../../data-types.md) | Y/N. Is the chat of this channel considered a group from the outside? Default: Y. By default, writing is not allowed. Leads and other CRM entities are also not created. | ||
|| **COMMENT**
[`unknown`](../../data-types.md) | Description for the embedding handler (see the PLACEMENT_HANDLER parameter). | ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

[*key_example]: 
```
data:image/svg+xml;charset=US-ASCII,%3Csvg%20version%3D%221.1%22%20id%3D%22Layer_1%22%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20x%3D%220px%22%20y%3D%220px%22%0A%09%20viewBox%3D%220%200%2070%2071%22%20style%3D%22enable-background%3Anew%200%200%2070%2071%3B%22%20xml%3Aspace%3D%22preserve%22%3E%0A%3Cpath%20fill%3D%22%230C99BA%22%20class%3D%22st0%22%20d%3D%22M34.7%2C64c-11.6%2C0-22-7.1-26.3-17.8C4%2C35.4%2C6.4%2C23%2C14.5%2C14.7c8.1-8.2%2C20.4-10.7%2C31-6.2%0A%09c12.5%2C5.4%2C19.6%2C18.8%2C17%2C32.2C60%2C54%2C48.3%2C63.8%2C34.7%2C64L34.7%2C64z%20M27.8%2C29c0.8-0.9%2C0.8-2.3%2C0-3.2l-1-1.2h19.3c1-0.1%2C1.7-0.9%2C1.7-1.8%0A%09v-0.9c0-1-0.7-1.8-1.7-1.8H26.8l1.1-1.2c0.8-0.9%2C0.8-2.3%2C0-3.2c-0.4-0.4-0.9-0.7-1.5-0.7s-1.1%2C0.2-1.5%2C0.7l-4.6%2C5.1%0A%09c-0.8%2C0.9-0.8%2C2.3%2C0%2C3.2l4.6%2C5.1c0.4%2C0.4%2C0.9%2C0.7%2C1.5%2C0.7C26.9%2C29.6%2C27.4%2C29.4%2C27.8%2C29L27.8%2C29z%20M44%2C41c-0.5-0.6-1.3-0.8-2-0.6%0A%09c-0.7%2C0.2-1.3%2C0.9-1.5%2C1.6c-0.2%2C0.8%2C0%2C1.6%2C0.5%2C2.2l1%2C1.2H22.8c-1%2C0.1-1.7%2C0.9-1.7%2C1.8v0.9c0%2C1%2C0.7%2C1.8%2C1.7%2C1.8h19.3l-1%2C1.2%0A%09c-0.5%2C0.6-0.7%2C1.4-0.5%2C2.2c0.2%2C0.8%2C0.7%2C1.4%2C1.5%2C1.6c0.7%2C0.2%2C1.5%2C0%2C2-0.6l4.6-5.1c0.8-0.9%2C0.8-2.3%2C0-3.2L44%2C41z%20M23.5%2C32.8%0A%09c-1%2C0.1-1.7%2C0.9-1.7%2C1.8v0.9c0%2C1%2C0.7%2C1.8%2C1.7%2C1.8h23.4c1-0.1%2C1.7-0.9%2C1.7-1.8v-0.9c0-1-0.7-1.8-1.7-1.9L23.5%2C32.8L23.5%2C32.8z%22/%3E%0A%3C/svg%3E%0A
```