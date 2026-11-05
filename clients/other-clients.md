# Other MCP clients

Elaichi keeps no list of approved MCP clients. Any client that supports OAuth dynamic client registration, PKCE with S256 and a browser sign-in can connect. Every client goes through the same registration, sign-in and consent. What differs is where the client's settings take the URL.

```text
https://api.elaichi.ai/mcp
```

The Elaichi team has not documented a test of clients beyond the ones in [the quick-start table](../README.md#quick-start). For any other client, check its docs for those three requirements and try it.

Elaichi's sign-in supports the authorization-code grant with PKCE, plus refresh. It has no device-code or client-credentials grant. A new grant always needs a person to approve the consent screen in a browser. An Elaichi API token or a pasted key does not work on the MCP endpoint.

Clients register at `POST https://api.elaichi.ai/oauth/register`. Registration is open and needs no Elaichi credential. A client's redirect address must match the one it registered exactly. The one exception is a loopback address (`localhost`, `127.0.0.1` or `[::1]`), which may use a different port.
