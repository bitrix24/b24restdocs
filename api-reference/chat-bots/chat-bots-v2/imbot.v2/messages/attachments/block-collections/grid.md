# Block for Rows and Columns GRID

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `GRID` block displays data in a tabular format of "name-value" pairs with various display options. It suits property cards: request status, priority, responsible person, deadline. For free text, use the [MESSAGE](./text.md) block.

Pass the block as an element of the attachment's `BLOCKS` array; the value of the `GRID` key is an array of elements. General attachment rules and limits are described on the [Attachments in Messages ATTACH](../index.md#limits) page.

## Display Options

The mode is set by the `DISPLAY` field of each element.

- `BLOCK` — each element is displayed as a separate block on a new line. Suits long values and descriptions
- `LINE` — elements are displayed in a single line as cards and wrap when there is insufficient width. Suits short labels and statuses
- `ROW` — two columns: `NAME` on the left, `VALUE` on the right. Suits a "field–value" property card
- `TABLE` — the API accepts the value, but the web client does not display an element in this mode, and the mobile app shows it as `BLOCK`. Use `ROW` instead of `TABLE`

If `DISPLAY` is not passed or is not recognized, the element is displayed as `BLOCK`. For compatibility, the legacy values `CARD` (same as `LINE`) and `COLUMN` (same as `ROW`) are accepted.

{% note warning %}

Set the same `DISPLAY` for all elements of a single `GRID` block. Clients render mixed modes differently: for example, the legacy web interface takes the mode of the whole block from the first element. If you need different modes, create several `GRID` blocks in a row.

{% endnote %}

## GRID Element Parameters

#|
|| **Name**
`type` | **Description** ||
|| **DISPLAY**
[`string`](../../../../../../data-types.md) | Display format: `BLOCK`, `LINE`, `ROW`. Defaults to `BLOCK` ||
|| **NAME**
[`string`](../../../../../../data-types.md) | Field name. In `ROW` mode, it may be omitted, in which case `VALUE` occupies the entire width of the row ||
|| **VALUE**
[`string`](../../../../../../data-types.md) | Field value, supports [BB codes](#bb-codes). In `ROW` mode, it may be omitted, in which case `NAME` occupies the entire width of the row. An element with both `NAME` and `VALUE` empty is skipped, except in `LINE` mode ||
|| **WIDTH**
[`integer`](../../../../../../data-types.md) | Width of the block or column in pixels ||
|| **HEIGHT**
[`integer`](../../../../../../data-types.md) | Height of the block in pixels. It is retained in the attachment, but the web client ignores it ||
|| **COLOR_TOKEN**
[`string`](../../../../../../data-types.md) | Color token for the value: `primary`, `secondary`, `alert`, `base`. Defaults to `base` ||
|| **COLOR**
[`string`](../../../../../../data-types.md) | HEX color of the value (`#RGB` or `#RRGGBB`). Only the legacy web interface applies it; current clients use `COLOR_TOKEN` ||
|| **LINK**
[`string`](../../../../../../data-types.md) | Link for the value: an absolute `http://` or `https://` URL or a path from the Bitrix24 root. Makes the entire value clickable. Clickable fragments inside the value are set with BB codes in `VALUE` ||
|| **USER_ID**
[`integer`](../../../../../../data-types.md) | User ID. It is retained in the attachment, but clients do not implement navigation by it. To link to a profile, pass the path `/company/personal/user/1/` in `LINK` ||
|| **CHAT_ID**
[`integer`](../../../../../../data-types.md) | Chat ID. It is retained in the attachment, but clients do not implement navigation by it ||
|#

## BB Codes in VALUE {#bb-codes}

`VALUE` supports the same set of BB codes as the [MESSAGE](./text.md#bb-codes) block, with the same differences between the web client and the mobile app.

An element whose value is a mention: `{"DISPLAY": "ROW", "NAME": "Assignee", "VALUE": "[USER=1]John Smith[/USER]"}`.

## Examples

{% include [Examples Note](../../../../../../../_includes/examples.md) %}

The examples show a single element of the `BLOCKS` array.

### Block Representation {#block-view}

`DISPLAY: 'BLOCK'` displays elements one below the other.

![Block Representation](./_images/grid1.png){width=420}

{% list tabs %}

- JS

    ```js
    {
        GRID: [
            {
                NAME: 'Description',
                VALUE: 'Implementation required to add structured objects to messages and notifications in the messenger.',
                DISPLAY: 'BLOCK',
                WIDTH: 250
            },
            {
                NAME: 'Category',
                VALUE: 'Requests',
                DISPLAY: 'BLOCK',
                WIDTH: 100
            }
        ]
    }
    ```

- Python

    ```python
    block = {
        "GRID": [
            {
                "NAME": "Description",
                "VALUE": "We need to implement the ability to add structured objects to messenger messages and notifications.",
                "DISPLAY": "BLOCK",
                "WIDTH": 250,
            },
            {
                "NAME": "Category",
                "VALUE": "Preferences",
                "DISPLAY": "BLOCK",
                "WIDTH": 100,
            },
        ],
    }
    ```

- PHP

    ```php
    [
        'GRID' => [
            [
                'NAME' => 'Description',
                'VALUE' => 'Implementation required to add structured objects to messages and notifications in the messenger.',
                'DISPLAY' => 'BLOCK',
                'WIDTH' => 250
            ],
            [
                'NAME' => 'Category',
                'VALUE' => 'Requests',
                'DISPLAY' => 'BLOCK',
                'WIDTH' => 100
            ]
        ]
    ]
    ```

{% endlist %}

### Line Representation {#inline-view}

`DISPLAY: 'LINE'` displays elements in a line, wrapping to the next line when there is insufficient space.

![Line Representation](./_images/grid2.png){width=420}

In the mobile version, elements are displayed one below the other.

{% list tabs %}

- JS

    ```js
    {
        GRID: [
            {
                NAME: 'Priority',
                VALUE: 'High',
                COLOR_TOKEN: 'alert',
                DISPLAY: 'LINE',
                WIDTH: 250
            },
            {
                NAME: 'Category',
                VALUE: 'Requests',
                DISPLAY: 'LINE'
            }
        ]
    }
    ```

- Python

    ```python
    block = {
        "GRID": [
            {
                "NAME": "Priority",
                "VALUE": "High",
                "COLOR_TOKEN": "alert",
                "DISPLAY": "LINE",
                "WIDTH": 250,
            },
            {
                "NAME": "Category",
                "VALUE": "Preferences",
                "DISPLAY": "LINE",
            },
        ],
    }
    ```

- PHP

    ```php
    [
        'GRID' => [
            [
                'NAME' => 'Priority',
                'VALUE' => 'High',
                'COLOR_TOKEN' => 'alert',
                'DISPLAY' => 'LINE',
                'WIDTH' => 250
            ],
            [
                'NAME' => 'Category',
                'VALUE' => 'Requests',
                'DISPLAY' => 'LINE'
            ]
        ]
    ]
    ```

{% endlist %}

### Two-Column Representation {#two-column-view}

`DISPLAY: 'ROW'` displays data in two columns.

![Two-Column Representation](./_images/grid3.png){width=420}

{% list tabs %}

- JS

    ```js
    {
        GRID: [
            {
                NAME: 'Priority',
                VALUE: 'High',
                DISPLAY: 'ROW'
            },
            {
                NAME: 'Category',
                VALUE: 'Requests',
                DISPLAY: 'ROW'
            }
        ]
    }
    ```

- Python

    ```python
    block = {
        "GRID": [
            {
                "NAME": "Priority",
                "VALUE": "High",
                "DISPLAY": "ROW",
            },
            {
                "NAME": "Category",
                "VALUE": "Preferences",
                "DISPLAY": "ROW",
            },
        ],
    }
    ```

- PHP

    ```php
    [
        'GRID' => [
            [
                'NAME' => 'Priority',
                'VALUE' => 'High',
                'DISPLAY' => 'ROW'
            ],
            [
                'NAME' => 'Category',
                'VALUE' => 'Requests',
                'DISPLAY' => 'ROW'
            ]
        ]
    ]
    ```

{% endlist %}

## Continue Learning

- [API imbot.v2 Change Log](../../../../change-log.md)
- [{#T}](./index.md)
- [{#T}](./text.md)
- [{#T}](./delimiter.md)
- [{#T}](../../message-formatting.md)
- [{#T}](../constructor.md)
- [{#T}](../../chat-message-send.md)