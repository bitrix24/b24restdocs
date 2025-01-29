# Object Site: Overview of Methods

The Object Site allows you to create and configure various web pages: a company business card, an online store, and much more.

You can retrieve a list of all sites in Bitrix24 using the method [landing.site.getList](./landing-site-get-list.md). To add a new site, use the method [landing.site.add](./landing-site-add.md).

> Quick navigation: [all methods](#all-methods)

## Site Structure

**Pages**. They store the site's content. Managed by a group of methods [landing.landing.*](../page/methods/index.md).
**Folders**. Optional. Used to group site pages and simplify navigation:
- create a new folder — method [landing.site.addFolder](./landing-site-add-folder.md),
- change folder parameters — method [landing.site.updateFolder](./landing-site-update-folder.md),
- get a list of all folders — method [landing.site.getFolders](./landing-site-get-folders.md).

{% note tip "User Documentation" %}

- [How to create a multi-page site](https://helpdesk.bitrix24.com/open/7410781/)
- [Site and page settings](https://helpdesk.bitrix24.com/open/21883080/)

{% endnote %}

## Linking Sites to Other Objects

**User**. The site is linked to users by numerical identifiers in the parameters `CREATED_BY_ID` and `MODIFIED_BY_ID`. You can obtain the user ID using the method [user.get](../../user/user-get.md).

## Actions with Sites

Sites can be updated using the method [landing.site.update](./landing-site-update.md). This method changes the title, description, homepage, color palette, and other parameters.

Site parameters can be exported to an array using the method [landing.site.fullExport](./landing-site-full-export.md). The resulting array should then be used in the method [landing.demos.register](../demos/landing-demos-register.md).

## How to Publish Site Content

To make the site accessible, call the method [landing.site.publication](./landing-site-publication.md). After publication, the site receives a unique URL, which can be obtained using the method [landing.site.getPublicUrl](./landing-site-get-public-url.md).

When you need to publish a specific folder, use the method [landing.site.publicationFolder](./landing-site-publication-folder.md).

## How to Unpublish a Site

You can unpublish:

- the entire site using the method [landing.site.unpublic](./landing-site-unpublic.md),
- a specific folder using the method [landing.site.unPublicFolder](./landing-site-unpublic-folder.md),
- a specific page using the method [landing.landing.unpublic](../page/methods/landing-landing-unpublic.md).

## How to Delete Folders and a Site

A specific folder can be moved to the trash using the method [landing.site.markFolderDelete](./landing-site-mark-folder-delete.md). If you need to move the entire site, use the method [landing.site.markDelete](./landing-site-mark-delete.md). Deleted folders and sites can be restored using the methods [landing.site.markFolderUnDelete](./landing-site-mark-folder-undelete.md) and [landing.site.markUnDelete](./landing-site-mark-undelete.md) within 30 days.

To delete a site without the possibility of recovery, use the method [landing.site.delete](./landing-site-delete.md). This will permanently destroy the site along with all folders and pages.

{% note tip "User Documentation" %}

- [Disabling and deleting sites](https://helpdesk.bitrix24.com/open/8232703/)

{% endnote %}

## Site Permissions

Access permissions allow you to control who can view and modify the site's content. Permissions are granted only by the portal administrator. Two permission models are supported:  
- role-based — permissions are configured using the method [landing.role.setRights](../rights/role-model/landing-role-set-rights.md),
- extended — permissions are configured using the method [landing.site.setRights](../rights/extended-model/landing-site-set-rights.md).

You can determine the model using the method [landing.role.isEnabled](../rights/landing-role-is-enabled.md).

{% note tip "User Documentation" %}

- [Site access permissions](https://helpdesk.bitrix24.com/open/22057418/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** | **Available since** ||
|| [landing.site.add](./landing-site-add.md) | Adds a site | ||
|| [landing.site.addFolder](./landing-site-add-folder.md) | Adds a folder to the site | 21.800.0 ||
|| [landing.site.delete](./landing-site-delete.md) | Deletes a site | ||
|| [landing.site.fullExport](./landing-site-full-export.md) | Exports the site and all its pages into a special array | ||
|| [landing.site.getFolders](./landing-site-get-folders.md) | Retrieves the site's folders | 21.800.0 ||
|| [landing.site.getList](./landing-site-get-list.md) | Retrieves a list of sites | ||
|| [landing.site.getPreview](./landing-site-get-preview.md) | Returns the preview image URL of the site | 21.800.0 ||
|| [landing.site.getPublicUrl](./landing-site-get-public-url.md) | Returns the full URL of the sites | 18.7.500 ||
|| [landing.site.getadditionalfields](./landing-site-get-additional-fields.md) | Retrieves additional fields of the site | ||
|| [landing.site.markDelete](./landing-site-mark-delete.md) | Marks the site as deleted | ||
|| [landing.site.markFolderDelete](./landing-site-mark-folder-delete.md) | Marks the folder as deleted | 21.800.0 ||
|| [landing.site.markFolderUnDelete](./landing-site-mark-folder-undelete.md) | Restores the folder from the trash | 21.800.0 ||
|| [landing.site.markUnDelete](./landing-site-mark-undelete.md) | Restores the site from the trash | ||
|| [landing.site.publication](./landing-site-publication.md) | Publishes the site and all its pages | ||
|| [landing.site.publicationFolder](./landing-site-publication-folder.md) | Publishes the site's folder | 21.800.0 ||
|| [landing.site.unPublicFolder](./landing-site-unpublic-folder.md) | Unpublishes the site's folder | 21.800.0 ||
|| [landing.site.unpublic](./landing-site-unpublic.md) | Unpublishes the site and all its pages | ||
|| [landing.site.update](./landing-site-update.md) | Updates site parameters | ||
|| [landing.site.updateFolder](./landing-site-update-folder.md) | Updates folder parameters | 21.800.0 ||
|#