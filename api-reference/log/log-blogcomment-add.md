# Add a comment to a news feed message log.blogcomment.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no success response
- no error response
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: any user

Adds a comment to the specified news feed message.

#|
|| **Parameter** | **Description** | **Version** ||
|| **USER_ID** | Comment author. A user with regular permissions cannot specify another user's ID as a value. This capability is available only to users with administrator rights | ||
|| **POST_ID** | Message ID | ||
|| **TEXT** | Comment text | ||
|| **FILES** | Files, an array of values described by the rules of [working with files](../how-to-call-rest-api/how-to-upload-files.md) | ||
|#

{% include [Footnote on parameters](../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    // Example of adding
    BX24.callMethod('log.blogcomment.add', {
        POST_ID: 10,
        TEXT: 'Comment on the post'
    }, result => {
        console.log(result);
    });
    ```

{% endlist %}

{% list tabs %}

- JS

    ```js
    // Retrieves a comment from the news feed. If no id is passed in the filter, it will return all comments available by permissions
    let params = {
        FIRST_ID: 893, //id from the table b_sonet_log_comment
        LAST_ID: 894,
    };
    BX24.callMethod('log.blogcomment.user.get', params,
        result => {
            if(result.error())
            {
                alert("Error: " + result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

{% endlist %}

{% list tabs %}

- JS

    ```js
    // Deletes a comment from the news feed
    let params = {
        COMMENT_ID: 261, //id from the table b_blog_comment
    };
    BX24.callMethod('log.blogcomment.delete', params,
        result => {
            if(result.error())
            {
                alert("Error: " + result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

{% endlist %}