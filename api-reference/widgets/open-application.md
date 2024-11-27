# Open a Slider with Your Interface

Often, there is a need in your applications to open a popup for user interaction. This could be a popup window where you want to display:

- application option settings,
- some dialog with the user,
- detailed information about a document, product, etc.

This is easy to achieve, but it's important to remember that the application interface is displayed within a frame. This means that any content you show is limited by the size of the frame. A large popup window will cause a scrollbar to appear on the frame, which is not always convenient and definitely not visually appealing.

To solve this problem, you are encouraged to use a slider. A slider is a popup window that opens over the frame and allows you to display any content without being constrained by the original frame size of the application. The Bitrix24 interface itself is built around the use of sliders, and you can use them in your applications.

## Example

```js
<script>
    
    BX24.openApplication(
        {
            'action': 'display_setting' // your custom data passed to the application for display in the slider
        },
        function()
        {
            // this handler will be triggered when the slider is closed
            alert('Application closed!')
        }
    );
   
</script>
```

You decide which parameters you want to pass to your application in order to generate the necessary content/interface for the slider on the application side. In the example above, we pass the parameter `action` with the value `display_setting` to the application. Upon receiving a POST request from Bitrix24 and processing this parameter, the application can create an interface with some user settings instead of showing the default application interface.

The request will come to the same URL that you specified in your application settings in the `Path to your handler` field for a [local application](../app-installation/local-apps/index.md), or in the `Application link` field for a [mass-market](../app-installation/mass-market-apps/index.md) application.

In other words, your code that responds to this URL should act as a controller or manager, generating different HTML based on the parameters received in the request.

## Advantages of Using a Slider

- **Visually Appealing**. Sliders in Bitrix24 are the standard way to display content, and users are accustomed to them.
- **Convenient**. Sliders allow you to show content of any size, without being limited by the application frame size, unlike popup windows.
- **Simple**. Understanding what interface to return in the slider is straightforward; it can be implemented with a simple condition in your code.
- **Flexible**. You decide what content to display in the slider and can change it based on the parameters passed in the request.