# Differences in Using REST API in Cloud and On-Premise Bitrix24

Bitrix24 exists both as a cloud service and as on-premise installations on the client side.

The advantage of REST applications is that they can operate in both environments in the same way, especially if certain nuances that may arise are considered during the application's development.

## Feature of On-Premise Bitrix24

In cloud Bitrix24, the REST API is always available in the "latest version." However, updates for the on-premise version may be released some time after they appear in the cloud due to the additional functionality that the on-premise Bitrix24 contains. Testing for compatibility with this functionality requires extra time.

Moreover, in the case of a specific on-premise installation, there is no guarantee that the owner has installed the latest available updates and has generally installed the necessary Bitrix24 modules for the application (for example, nothing prevents the telephony module from being completely disabled in on-premise Bitrix24). This means that when installing the application, there is no certainty about the availability of specific REST methods.

Therefore, **recommendation number 1** — in your solution's installation script, check the list of available methods and events by referring to the [methods](../common/system/methods.md) and [events](../events/events.md), and inform the user that they need to install updates for Bitrix24.

## Bitrix24 Authorization Server

If your application refreshes authorization tokens using `refresh_token`, there are two options: obtain a new pair of tokens by making a request to

```
xxx.bitrix24.xxx/oauth/token/?grant_type=authorization_code...
```

or to the authorization server

```
oauth.bitrix.info/oauth/token/?grant_type=authorization_code...
```

In the case of cloud Bitrix24, both options will work equally well, as cloud Bitrix24 will simply "forward" your request to the actual authorization server; however, on-premise Bitrix24 may not do this.

Therefore, **recommendation number 2** — always address the authorization server directly for token refreshes. This will work for both cloud and on-premise.

## Network Limitations

When accessing REST methods, your application makes a request to a specific Bitrix24. However, REST events do not come directly from a specific Bitrix24 to your application; they go through an event queue located in the cloud, even if it concerns on-premise Bitrix24.

It is possible that the owner of the on-premise Bitrix24 has closed network access to the event queue server for some reason. In this case, events simply will not work. If you are implementing a solution while communicating with the client, you should recommend whitelisting the queue server's address.

The same applies to the authorization server. If network access to it is closed, REST will not work in on-premise Bitrix24, as on-premise Bitrix24 will not be able to validate the token that your application is using to access it.

There is an alternative option where you can specifically connect your own authorization and event mechanism for a particular client on their on-premise Bitrix24. 