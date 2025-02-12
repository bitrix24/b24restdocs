# About REST API in Websites

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards

{% endnote %}

{% endif %}

> Quick navigation: [all methods](#all-methods)

To create a fully functional website via REST or to make changes to an existing one, you need to understand that REST mimics user interaction logic. For instance, to start modifying a block, you must first add it if it's not already on the page, and to make the changes visible, you need to publish the page. But let's summarize the steps briefly.

So, "to create a website," you need just a few things:

1. Create a website or choose one from the existing ones. In the end, you will have an identifier for the website you are working with. ([Methods](./site/index.md) for working with the website.)
2. Now it's time for the page. Similarly, create a page or select from existing ones. ([Methods](./page/index.md) for working with the page.)
3. Blocks. Blocks are the molecules of websites (nodes – atoms). You should have a good understanding of what a [block](./block/index.md) is and what its [manifest](./block/manifest.md) is. You can work with blocks in terms of the page (adding, moving, deleting) using [these methods](./page/block-methods/index.md). However, to work with a specific block, use [these methods](./block/methods/index.md).
4. Don't forget, after all actions, the page needs to be [published](./page/methods/landing-landing-publication.md).
5. If you need more blocks, you can always [register new ones](./demos/index.md).

This is the essential framework you need to work with blocks. There are certainly many more methods, and they are quite specific to cover most of your use cases.

**Good luck!**

## Overview of Methods {#all-methods}

### User Blocks

#|
|| **Method** | **Description** | **Version** ||
|| [landing.repo.getList](./user-blocks/landing-repo-get-list.md) | Method to get the list of blocks in the current application. | ||
|| [landing.repo.register](./user-blocks/landing-repo-register.md) | Method to add a block to the repository. | ||
|| [landing.repo.unregister](./user-blocks/landing-repo-unregister.md) | Method to remove a block. | ||
|| [landing.repo.checkContent](./user-blocks/landing-repo-check-content.md) | Method checks content for dangerous substrings. | ||
|#

### Templates

#|
|| **Method** | **Description** ||
|| [landing.template.getLandingRef](./template/landing-template-get-landing-ref.md) | Gets the list of included areas for the page. ||
|| [landing.template.getlist](./template/landing-template-get-list.md) | Gets the list of templates. ||
|| [landing.template.getSiteRef](./template/landing-template-get-site-ref.md) | Gets the list of included areas for the site. ||
|| [landing.template.setLandingRef](./template/landing-template-set-landing-ref.md) | Sets the included areas for the page. ||
|| [landing.template.setSiteRef](./template/landing-template-set-site-ref.md) | Sets the included areas for the site. ||
|#

### User Templates

#|
|| **Method** | **Description** ||
|| [landing.demos.register](./demos/landing-demos-register.md) | Method registers a template in the site and page creation wizard. ||
|| [landing.demos.unregister](./demos/landing-demos-unregister.md) | Method removes a registered user template. ||
|| [landing.demos.getList](./demos/landing-demos-get-list.md) | Method to get the list of available user templates in the current application. ||
|| [landing.demos.getSiteList](./demos/landing-demos-get-site-list.md) | Method to get the list of available templates for creating websites. ||
|| [landing.demos.getPageList](./demos/landing-demos-get-page-list.md) | Method to get the list of available templates for creating pages. ||
|#

### Websites

#|
|| **Method** | **Description** | **Version** ||
|| [landing.site.add](./site/landing-site-add.md) | Adds a website. | ||
|| [landing.site.addFolder](./site/landing-site-add-folder.md) | Adds a folder to the website. | 21.800.0 ||
|| [landing.site.delete](./site/landing-site-delete.md) | Deletes a website. | ||
|| [landing.site.fullExport](./site/landing-site-full-export.md) | Exports the website and all its pages into a special array. | ||
|| [landing.site.getFolders](./site/landing-site-get-folders.md) | Gets the folders of the website. | 21.800.0 ||
|| [landing.site.getList](./site/landing-site-get-list.md) | Gets the list of websites. | ||
|| [landing.site.getPreview](./site/landing-site-get-preview.md) | Returns the URL of the website's preview image. | 21.800.0 ||
|| [landing.site.getPublicUrl](./site/landing-site-get-public-url.md) | Returns the full URL of the websites. | 18.7.500 ||
|| [landing.site.getadditionalfields](./site/landing-site-get-additional-fields.md) | Gets additional fields of the website. | ||
|| [landing.site.markDelete](./site/landing-site-mark-delete.md) | Marks the website as deleted. | ||
|| [landing.site.markFolderDelete](./site/landing-site-mark-folder-delete.md) | Marks the folder as deleted. | 21.800.0 ||
|| [landing.site.markFolderUnDelete](./site/landing-site-mark-folder-undelete.md) | Restores the folder from the trash. | 21.800.0 ||
|| [landing.site.markUnDelete](./site/landing-site-mark-undelete.md) | Restores the website from the trash. | ||
|| [landing.site.publication](./site/landing-site-publication.md) | Publishes the website and all its pages. | ||
|| [landing.site.publicationFolder](./site/landing-site-publication-folder.md) | Publishes the website folder. | 21.800.0 ||
|| [landing.site.unPublicFolder](./site/landing-site-unpublic-folder.md) | Unpublishes the website folder. | 21.800.0 ||
|| [landing.site.unpublic](./site/landing-site-unpublic.md) | Unpublishes the website and all its pages. | ||
|| [landing.site.update](./site/landing-site-update.md) | Updates the website parameters. | ||
|| [landing.site.updateFolder](./site/landing-site-update-folder.md) | Updates the folder parameters. | 21.800.0 ||
|#

### Rights

#|
|| **Method** | **Description** ||
|| [landing.role.enable](./rights/landing-role-enable.md) | Toggles models. ||
|| [landing.role.isEnabled](./rights/landing-role-is-enabled.md) | Determines the rights models. ||
|#

#### Extended Rights Model

#|
|| **Method** | **Description** ||
|| [landing.site.getRights](./rights/extended-model/landing-site-get-rights.md) | Method returns the rights of the current user. ||
|| [landing.site.setRights](./rights/extended-model/landing-site-set-rights.md) | Sets access permissions for the website. ||
|#

#### Role-Based Rights Model

#|
|| **Method** | **Description** ||
|| [landing.role.getList](./rights/role-model/landing-role-get-list.md) | Method allows getting the list of roles. ||
|| [landing.role.getRights](./rights/role-model/landing-role-get-rights.md) | Method allows getting the list of websites for which rights are set within the role. ||
|| [landing.role.setAccessCodes](./rights/role-model/landing-role-set-access-codes.md) | Method sets access codes for the role that will apply to this role. ||
|| [landing.role.setRights](./rights/role-model/landing-role-set-rights.md) | Method sets the necessary rights within the role for website lists. ||
|#

### Pages

#|
|| **Method** | **Description** | **Version** ||
|| [landing.landing.add](./page/methods/landing-landing-add.md) | Method for adding a page. | ||
|| [landing.landing.addByTemplate](./page/methods/landing-landing-add-by-template.md) | Method for adding a page by template. | ||
|| [landing.landing.copy](./page/methods/landing-landing-copy.md) | Method copies the specified page. | ||
|| [landing.landing.delete](./page/methods/landing-landing-delete.md) | Method for deleting a page. | ||
|| [landing.landing.getadditionalfields](./page/methods/landing-landing-get-additional-fields.md) | Method for getting additional fields of the page. | ||
|| [landing.landing.getlist](./page/methods/landing-landing-get-list.md) | Method for getting the list of pages. | ||
|| [landing.landing.getpreview](./page/methods/landing-landing-get-preview.md) | Method returns the path to the page preview. | ||
|| [landing.landing.getpublicurl](./page/methods/landing-landing-get-public-url.md) | Method returns the web address of the page. | ||
|| [landing.landing.markDelete](./page/methods/landing-landing-mark-delete.md) | Method marks the page as deleted. | ||
|| [landing.landing.markUnDelete](./page/methods/landing-landing-mark-undelete.md) | Method marks the page as not deleted. | ||
|| [landing.landing.move](./page/methods/landing-landing-move.md) | Method moves the page to another website and/or folder. | 21.800.0 ||
|| [landing.landing.publication](./page/methods/landing-landing-publication.md) | Method for publishing the page. | ||
|| [landing.landing.removeEntities](./page/methods/landing-landing-remove-entities.md) | Method removes related entities of the landing. | ||
|| [landing.landing.resolveIdByPublicUrl](./page/methods/landing-landing-resolve-id-by-public-url.md) | Method returns the page identifier by the provided relative URL. | 21.800.0 ||
|| [landing.landing.unpublic](./page/methods/landing-landing-unpublic.md) | Method for unpublishing the page. | ||
|| [landing.landing.update](./page/methods/landing-landing-update.md) | Method for modifying the page. | ||
|#

#### Working with Blocks on the Page

#|
|| **Method** | **Description** | **Version** ||
|| [landing.landing.addblock](./page/block-methods/landing-landing-add-block.md) | Method for adding a new block to the page. | ||
|| [landing.landing.copyblock](./page/block-methods/landing-landing-copy-block.md) | Method for copying a block from one page to another. | ||
|| [landing.landing.deleteblock](./page/block-methods/landing-landing-delete-block.md) | Method for deleting a block from the page. | ||
|| [landing.landing.downblock](./page/block-methods/landing-landing-down-block.md) | Method for moving a block down one position on the page. | ||
|| [landing.landing.favoriteBlock](./page/block-methods/landing-landing-favorite-block.md) | Method saves an existing block on the page to "My Blocks." | 21.800.0 ||
|| [landing.landing.hideblock](./page/block-methods/landing-landing-hide-block.md) | Method hides a block from the page. | ||
|| [landing.landing.markdeletedblock](./page/block-methods/landing-landing-mark-deleted-block.md) | Method marks a block as deleted but does not physically remove it. | ||
|| [landing.landing.markundeletedblock](./page/block-methods/landing-landing-mark-undeleted-block.md) | Method restores a block from the marked as deleted state. | ||
|| [landing.landing.moveblock](./page/block-methods/landing-landing-move-block.md) | Method for moving a block from one page to another. | ||
|| [landing.landing.showblock](./page/block-methods/landing-landing-show-block.md) | Method for showing a block on the page. | ||
|| [landing.landing.unFavoriteBlock](./page/block-methods/landing-landing-unfavorite-block.md) | Method removes a block that was saved in "My Blocks." | 21.800.0 ||
|| [landing.landing.upblock](./page/block-methods/landing-landing-up-block.md) | Method for moving a block up one position on the page. | ||
|#

#### Special Pages

#|
|| **Method** | **Description** ||
|| [landing.syspage.deleteForLanding](./page/special-pages/landing-syspage-delete-for-landing.md) | Removes all mentions of the page as special. ||
|| [landing.syspage.deleteForSite](./page/special-pages/landing-syspage-delete-for-site.md) | Removes all special pages. ||
|| [landing.syspage.getSpecialPage](./page/special-pages/landing-syspage-get-special-page.md) | Gets the address of the special page of the site. ||
|| [landing.syspage.get](./page/special-pages/landing-syspage-get.md) | Gets the list of special pages. ||
|| [landing.syspage.set](./page/special-pages/landing-syspage-set.md) | Sets a special page for the site. ||
|#

### Blocks

#|
|| **Method** | **Description** | **Version** ||
|| [landing.block.clonecard](./block/methods/landing-block-clone-card.md) | Method for cloning a block card. | ||
|| [landing.block.removecard](./block/methods/landing-block-remove-card.md) | Method for removing a block. | ||
|| [landing.block.updatenodes](./block/methods/landing-block-update-nodes.md) | Method for changing the content of a block. | ||
|| [landing.block.changeNodeName](./block/methods/landing-block-change-node-name.md) | Method changes the tag name. | ||
|| [landing.block.updateattrs](./block/methods/landing-block-update-attrs.md) | Method for changing the attributes of a block node. | ||
|| [landing.block.updateStyles](./block/methods/landing-block-update-styles.md) | Method for changing the styles of a block. | ||
|| [landing.block.getcontent](./block/methods/landing-block-get-content.md) | Method for getting the content of a block. | ||
|| [landing.block.getlist](./block/methods/landing-block-get-list.md) | Method for getting the list of blocks on the page. | ||
|| [landing.block.getbyid](./block/methods/landing-block-get-by-id.md) | Method for getting a block by its identifier. | ||
|| [landing.block.getmanifest](./block/methods/landing-block-get-manifest.md) | Method for getting the manifest of a specific block already placed on the page. | ||
|| [landing.block.getmanifestfile](./block/methods/landing-block-get-manifest-file.md) | Method for getting the manifest of a block from the repository. | ||
|| [landing.block.getrepository](./block/methods/landing-block-get-repository.md) | Method returns the list of blocks from the repository. | ||
|| [landing.block.uploadfile](./block/methods/landing-block-upload-file.md) | Method uploads an image and associates it with the specified block. | ||
|| [landing.block.updatecontent](./block/methods/landing-block-update-content.md) | Method updates the content of a block already placed on the page to any arbitrary content. | ||
|| [landing.block.addcard](./block/methods/landing-block-add-card.md) | Method fully replicates the work of [landing.block.clonecard](./block/methods/landing-block-clone-card.md) but allows inserting a card with modified content immediately. | ||
|| [landing.block.updateCards](./block/methods/landing-block-update-cards.md) | Method for mass updating block cards. | ||
|| [landing.block.changeAnchor](./block/methods/landing-block-change-anchor.md) | Method changes the symbolic code of the anchor. | ||
|| [landing.block.getContentFromRepository](./block/methods/landing-block-get-content-from-repository.md) | Method gets the content of a block from the repository "as is" before adding the block to any page. | 18.7.500 ||
|#

### Embedding Knowledge Base

#|
|| **Method** | **Description** ||
|| [landing.site.bindingToGroup](./embedding/knowledge-base/landing-site-binding-to-group.md) | Binds to a social network group. ||
|| [landing.site.bindingToMenu](./embedding/knowledge-base/landing-site-binding-to-menu.md) | Embeds in the menu. ||
|| [landing.site.getGroupBindings](./embedding/knowledge-base/landing-site-get-group-bindings.md) | Gets bindings to groups. ||
|| [landing.site.getMenuBindings](./embedding/knowledge-base/landing-site-get-menu-bindings.md) | Gets the list of bindings in the menu. ||
|| [landing.site.unbindingFromGroup](./embedding/knowledge-base/landing-site-unbinding-from-group.md) | Removes the binding to a social network group. ||
|| [landing.site.unbindingFromMenu](./embedding/knowledge-base/landing-site-unbinding-from-menu.md) | Removes from the menu. ||
|#