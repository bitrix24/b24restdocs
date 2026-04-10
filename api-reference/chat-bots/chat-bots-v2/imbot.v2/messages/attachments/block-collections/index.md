# ATTACH Block Collection

Blocks define the structure and appearance of the `ATTACH` attachment.

## Block Types

### [User Block (USER)](./user.md)

Displays the user card within the attachment: name, avatar, and a link to the profile or external resource.

![User Block](./_images/user.png){width=420}

### [Link Block (LINK)](./links.md)

Adds a clickable link with a caption. Suitable for navigating to a task, document, deal, or external page.

![Link Block](./_images/link.png){width=420}

### [Text Block (MESSAGE)](./text.md)

Outputs a text fragment of the attachment. Used for headings, explanations, comments, and main content.

![Text Block](./_images/text.png){width=420}

### [Delimiter Block (DELIMITER)](./delimiter.md)

Adds a visual separator between parts of the attachment. Helps to distinguish meaningful blocks in a long card.

![Delimiter Block](./_images/delimiter.png){width=420}

### [Grid Block for Rows and Columns (GRID)](./grid.md)

Forms a tabular structure from pairs of "name-value". Suitable for cards with properties and parameters.

1. [Block Representation (BLOCK)](./grid.md#block-representation)

   ![Block Construction](./_images/grid1.png){width=420}

2. [Line Representation (LINE)](./grid.md#line-representation)

   ![Line Construction](./_images/grid2.png){width=420}

   In the mobile version, blocks are displayed one below the other.

3. [Two-Column Representation (ROW)](./grid.md#two-column-representation)

   ![Two-Column Construction](./_images/grid3.png){width=420}

### [Image Block (IMAGE)](./images.md)

Displays one or more images within the attachment.

![Image Block](./_images/img.png){width=420}

### [File Block (FILE)](./files.md)

Adds a file with a name and a link for downloading or opening.

![File Block](./_images/file.png){width=420}

## Continue Exploring

- [API Change Log for imbot.v2](../../../../change-log.md)
- [{#T}](../index.md)
- [{#T}](../constructor.md)
- [Working with Keyboards](../../message-keyboards.md) — buttons under the message for commands, links, and actions