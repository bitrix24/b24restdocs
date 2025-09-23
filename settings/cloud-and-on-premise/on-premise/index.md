# Features of the REST API in the On-Premise Version of Bitrix24

In the cloud version of Bitrix24, the REST API is always available in the "latest version." However, updates for the on-premise version may be released some time after they appear in the cloud, due to the additional functionality in the on-premise Bitrix24 that requires extra time for compatibility testing.

Moreover, in the case of a specific on-premise installation, there is no guarantee that the owner has installed the latest available updates or the necessary modules for the application in Bitrix24 (for example, nothing prevents the owner from completely disabling the telephony module in the on-premise Bitrix24). This means that when installing an application, there is no certainty about the availability of specific REST methods.

Therefore, **recommendation number 1** in your installation script is to check the list of available methods and events by referring to the [methods](../../../api-reference/common/system/methods.md) and [events](../../../api-reference/events/index.md) documentation, and inform the user that they need to install updates for Bitrix24.

## Bitrix24 Authorization Server

If your application refreshes authorization tokens using `refresh_token`, there are two options: obtain a new pair of tokens by making a request to

```
xxx.bitrix24.xxx/oauth/token/?grant_type=authorization_code...
```

or to the authorization server

```
oauth.bitrix.info/oauth/token/?grant_type=authorization_code...
```

In the case of cloud Bitrix24, both options will work equally well, as the cloud Bitrix24 will simply "forward" your request to the actual authorization server; however, the on-premise Bitrix24 may not do this.

Therefore, **recommendation number 2** is to always address the authorization server directly for token updates. This will work for both the cloud and the on-premise versions.

## Network Limitations

When accessing REST methods, your application makes a request to a specific Bitrix24. However, REST events do not come directly from a specific Bitrix24 to your application; instead, they go through an event queue located in the cloud, even if it concerns the on-premise Bitrix24.

There may be a situation where the owner of the on-premise Bitrix24 has closed network access to the event queue server for some reason. In this case, events simply will not work. If you are implementing a solution while communicating with the client, you should recommend whitelisting the address of the queue server.

The same applies to the authorization server. If network access to it is closed, then REST will not work in the on-premise Bitrix24, as the on-premise Bitrix24 will not be able to validate the token that your application is using to access it.

There is an alternative option where you can specifically connect your own authorization and event mechanism for a particular client on their on-premise Bitrix24. More details about this method were discussed in a presentation by Maxim Sidorenko:

@[youtube](https://www.youtube.com/watch?v=MtTVF9Vf0Wo)