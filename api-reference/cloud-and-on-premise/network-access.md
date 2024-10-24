# Required Network Access

In the case of using the on-premise version of Bitrix24, the company's network administrator must consider the need to open access for incoming/outgoing requests from REST applications in the security policy.

For Bitrix24, the following must be opened:

- Outgoing requests to **oauth.bitrix.info** (for the application mechanism)
- Access to **\*.bitrixsoft.com**. Without access to this resource, the "Developer resources" section for creating integrations and webhooks will not function
- Incoming requests from application servers (the addresses depend on the specific applications)

If the application is being developed on its own server, the following must be opened:

- Outgoing HTTPS requests to **oauth.bitrix.info**
- Outgoing HTTPS requests to the specific server of the on-premise Bitrix24
- Incoming HTTP/S requests from the server group **mp_actions-\***, if the application uses event mechanisms, automation rules, or custom workflow actions

Incoming HTTP/S requests may come from a dynamic (scale-based) group of servers with different IP addresses. A list of such IP addresses can be obtained in advance by querying [https://dl.bitrix24.com/webhook/app-world.json](https://dl.bitrix24.com/webhook/app-world.json).

Example request using **curl**:

```bash
$ curl https://dl.bitrix24.com/webhook/app-world.json
{
    "nodes": ["3.217.33.54", "52.29.163.104"]
}
```

The list of IP addresses received in the `nodes` array should be used to modify the firewall rules for incoming connections in the corporate network or on the on-premise VM.

The polling frequency for the above URI should be a maximum of once per minute, but it is better to do it once every 5-10 minutes. The scaling mechanism of the VM pool will ensure that 5-10 minutes before webhooks start arriving from a new address, it will already be present in the `nodes` list.

{% note info %}

Previously, the company "1C-Bitrix" specified specific IPs that needed to be opened for requests. However, since these addresses change, it is better to rely on the IP addresses that resolve the specified domain names.

{% endnote %}