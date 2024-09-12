# Block for Building Rows and Columns GRID

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard

{% endnote %}

{% endif %}

`GRID` - a block for displaying information in a table format, which can be presented in three forms: block, line, and two-column.

{% note warning %}

In all three views, mixing elements within a single entry is not allowed. If you need to use different types of blocks simultaneously, you must create a new block.

{% cut "Examples" %}

{% list tabs %}

- JS

    ```js
    {
        GRID: [
            {
                NAME: "Priority",
                VALUE: "High",
                DISPLAY: "COLUMN",
            },
            {
                NAME: "Category",
                VALUE: "Requests",
                DISPLAY: "COLUMN",
            },
        ]
    },
    {
        GRID: [
            {
                NAME: "Description",
                VALUE: "It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.",
                DISPLAY: "BLOCK",
                WIDTH: 250
            },
            {
                NAME: "Category",
                VALUE: "Requests",
                DISPLAY: "BLOCK",
                WIDTH: 100
            },
        ]
    },
    ```

- PHP

    ```php
    Array(
        "GRID" => Array(
            Array(
                "NAME" => "Priority",
                "VALUE" => "High",
                "DISPLAY" => "COLUMN",
            ),
            Array(
                "NAME" => "Category",
                "VALUE" => "Requests",
                "DISPLAY" => "COLUMN"
            ),
        )
    ),
    Array(
        "GRID" => Array(
            Array(
                "NAME" => "Description",
                "VALUE" => "It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.",
                "DISPLAY" => "BLOCK",
                "WIDTH" => "250"
            ),
            Array(
                "NAME" => "Category",
                "VALUE" => "Requests",
                "DISPLAY" => "BLOCK"
            ),
        )
    ),
    ```

{% endlist %}

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% endcut %}

{% endnote %}

For all types of blocks for building rows and columns, additional keys are available:
- **COLOR_TOKEN** — token for the value color. Can take one of the following values:
  - `primary`
  - `secondary`
  - `alert`
  - `base`
By default, it has the value `base`
- **COLOR** — color of the value;
- **CHAT_ID** — the value will become a link to the chat in the web messenger;
- **USER_ID** — the value will become a link to the user in the web messenger;
- **LINK** — the value will become a link.

For the **VALUE** key, bb-codes are available: `USER`, `CHAT`, `SEND`, `PUT`, `CALL`, `BR`, `B`, `U`, `I`, `S`, `URL`.

## Block View

`"DISPLAY" => "BLOCK"` — data is displayed one below the other. The **WIDTH** key is available to specify the width of the block (in pixels).

### Example

{% list tabs %}

- JS

    ```js
    {
        GRID: [
            {
                NAME: "Description",
                VALUE: "It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.",
                DISPLAY: "BLOCK",
                WIDTH: 250
            },
            {
                NAME: "Category",
                VALUE: "Requests",
                DISPLAY: "BLOCK",
                WIDTH: 100
            },
        ]
    },
    ```

- PHP

    ```php
    Array(
        "GRID" => Array(
            Array(
                "NAME" => "Description",
                "VALUE" => "It is necessary to implement the ability to add structured entities to messages and notifications in the messenger.",
                "DISPLAY" => "BLOCK",
                "WIDTH" => "250"
            ),
            Array(
                "NAME" => "Category",
                "VALUE" => "Requests",
                "DISPLAY" => "BLOCK"
            ),
        )
    ),
    ```

{% endlist %}

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

## Line View

`"DISPLAY" => "LINE"` - each block follows one after another until the available space runs out, after which it moves to a new line. The **WIDTH** key is available to specify the width of the block (in pixels).

### Example

{% list tabs %}

- JS

    ```js
    {
        GRID: [
            {
                NAME: "Priority",
                VALUE: "High",
                COLOR_TOKEN: "alert",
                COLOR: "#ff0000",
                DISPLAY: "LINE",
                WIDTH: 250
            },
            {
                NAME: "Category",
                VALUE: "Requests",
                DISPLAY: "LINE",
            },
        ]
    },
    ```

- PHP

    ```php
    Array(
        "GRID" => Array(
            Array(
                "NAME" => "Priority",
                "VALUE" => "High",
                "COLOR_TOKEN" => "alert",
                "COLOR" => "#ff0000",
                "DISPLAY" => "LINE",
                "WIDTH" => "250"
            ),
            Array(
                "NAME" => "Category",
                "VALUE" => "Requests",
                "DISPLAY" => "LINE"
            ),
        )
    ),
    ```

{% endlist %}

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

## Two-Column View

`"DISPLAY" => "COLUMN"` - builds in two columns. The **WIDTH** key is available to specify the width of the first column (in pixels).

{% note info %}

Starting from version 22 of REST, in the two-column view, you can omit one of the required parameters **NAME** or **VALUE**:
- if **VALUE** is not provided, **NAME** will take the full width of the table;
- if **NAME** is not provided, **VALUE** will take the full width of the column.

{% endnote %}

### Example

{% list tabs %}

- JS

    ```js
    {
        GRID: [
            {
                NAME: "Priority",
                VALUE: "High",
                DISPLAY: "COLUMN",
            },
            {
                NAME: "Category",
                VALUE: "Requests",
                DISPLAY: "COLUMN",
            },
        ]
    },
    ```

- PHP

    ```php
    Array(
        "GRID" => Array(
            Array(
                "NAME" => "Priority",
                "VALUE" => "High",
                "DISPLAY" => "COLUMN",
                "WIDTH" => "250"
            ),
            Array(
                "NAME" => "Category",
                "VALUE" => "Requests",
                "DISPLAY" => "COLUMN"
            ),
        )
    ),
    ```

{% endlist %}

{% include [Footnote on examples](../../../../../_includes/examples.md) %}