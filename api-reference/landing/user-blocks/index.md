# Custom Blocks: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom blocks are added to the application's repository for websites and pages.

The group of methods `landing.repo.*` allows you to work with custom blocks. In this section, you can check the content, register a block in the repository, retrieve a list of custom blocks, and remove any unnecessary blocks.

> Quick Navigation: [All Methods](#all-methods)

## Getting Started

1. Prepare the HTML for the block, its preview, and its main fields.
2. If a manifest is required for the block, prepare it according to the article [Manifest File](../block/manifest.md).
3. If you need to validate the HTML before registration, use the method [landing.repo.checkContent](./landing-repo-check-content.md).
4. Register the block using the method [landing.repo.register](./landing-repo-register.md).
5. Retrieve the list of custom blocks and their codes using the method [landing.repo.getList](./landing-repo-get-list.md).
6. Remove any unnecessary blocks using the method [landing.repo.unregister](./landing-repo-unregister.md).

## Key Parameters

**code.** A unique code for the block. It is used during registration and deletion. If you register a block again with the same `code`, the method [landing.repo.register](./landing-repo-register.md) will update the existing block of the current application.

**XML_ID.** The external code of the block returned by [landing.repo.getList](./landing-repo-get-list.md). It can be used as the value for the `code` parameter when deleting a block via [landing.repo.unregister](./landing-repo-unregister.md).

**CONTENT.** The HTML content of the block. The `CONTENT` field undergoes a sanitize check during registration. For preliminary HTML validation, you can use [landing.repo.checkContent](./landing-repo-check-content.md).

**SECTIONS.** Categories of the block as a comma-separated string. The value is passed in the `fields` of the method [landing.repo.register](./landing-repo-register.md). Example: `cover,about`.

**PREVIEW.** The URL of the block's preview. The value is passed in the `fields` of the method [landing.repo.register](./landing-repo-register.md).

**SITE_TEMPLATE_ID.** The binding of the block to a specific site template. The value is passed in the `fields` of the method [landing.repo.register](./landing-repo-register.md) if the block is intended to be used only with a specific template.

**RESET.** A flag indicating the update of blocks with the code `repo_<ID>` that have already been added to the pages. The value is passed in the `fields` of the method [landing.repo.register](./landing-repo-register.md).

## Relationship with Other Objects

**Landing Blocks.** A custom block is built on the same principles as landing blocks. When registering, you can pass the `manifest` parameter.

Refer to the manifest structure in the method [landing.block.getManifestFile](../block/methods/landing-block-get-manifest-file.md) and in the article [Manifest File](../block/manifest.md).

**Application Repository.** The methods `landing.repo.*` work with blocks that the application adds to its own repository. The list of such blocks is returned by [landing.repo.getList](./landing-repo-get-list.md).

**Site Template.** If the block needs to be restricted to a specific site template, the `SITE_TEMPLATE_ID` parameter is used during registration in the method [landing.repo.register](./landing-repo-register.md).

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with View permission in the Sites section

#| 
|| **Method** | **Description** ||
|| [landing.repo.register](./landing-repo-register.md) | Registers a custom block in the repository ||
|| [landing.repo.checkContent](./landing-repo-check-content.md) | Checks the block's content through the sanitizer ||
|| [landing.repo.getList](./landing-repo-get-list.md) | Retrieves a list of custom blocks from the repository ||
|| [landing.repo.unregister](./landing-repo-unregister.md) | Removes a custom block from the repository by its code ||
|#