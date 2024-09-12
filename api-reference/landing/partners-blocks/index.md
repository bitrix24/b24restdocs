# Partner Blocks

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

Description of the partner blocks repository and methods for working with it.

The partner repository operates on the same principles as the [standard blocks](../block/index.md). The partner repository allows you to add your own blocks to the local catalog (accessible only on the current account) with virtually no restrictions.

## List of Methods

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** | **Version** ||
|| [landing.repo.getList](./landing-repo-get-list.md) | Method to retrieve the list of blocks in the current application. | ||
|| [landing.repo.register](./landing-repo-register.md) | Method to add a block to the repository. | ||
|| [landing.repo.unregister](./landing-repo-unregister.md) | Method to remove a block. | ||
|| [landing.repo.checkContent](./landing-repo-check-content.md) | Method checks content for dangerous substrings. | ||
|#