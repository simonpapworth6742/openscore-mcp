# Publishing the listing

How `server.json` gets into the official MCP Registry and the community directories. The server is already live; these steps only list it.

## Official MCP Registry

The server is named `uk.org.openscore/care-homes`. The registry accepts that name only from someone who proves they control `openscore.org.uk`, by adding a DNS TXT record to the domain.

Run these in **Git Bash** (it includes OpenSSL), from this folder.

### 1. Make a signing key and the TXT record (once)

```bash
openssl genpkey -algorithm Ed25519 -out key.pem
PUBLIC_KEY="$(openssl pkey -in key.pem -pubout -outform DER | tail -c 32 | base64)"
echo "v=MCPv1; k=ed25519; p=${PUBLIC_KEY}"
```

Keep `key.pem` somewhere safe and private (a password manager is fine). It is in `.gitignore` and must never be committed. Anyone with it can publish under the OpenScore name.

### 2. Add the TXT record

In the DNS settings for `openscore.org.uk`, add:

- Type: `TXT`
- Name / host: `@` (the bare domain, **not** `_mcp-auth` or any other prefix)
- Value: the `v=MCPv1; k=ed25519; p=...` line printed above

Wait a few minutes, then check it is visible:

```bash
nslookup -type=TXT openscore.org.uk
```

### 3. Get the publisher tool

Download the Windows build of `mcp-publisher` from the [registry releases page](https://github.com/modelcontextprotocol/registry/releases) and put `mcp-publisher.exe` in this folder.

### 4. Log in and publish

```bash
PRIVATE_KEY="$(openssl pkey -in key.pem -noout -text | grep -A3 "priv:" | tail -n +2 | tr -d ' :\n')"
./mcp-publisher login dns --domain openscore.org.uk --private-key "${PRIVATE_KEY}"
./mcp-publisher publish
```

Check the listing:

```bash
curl "https://registry.modelcontextprotocol.io/v0/servers?search=uk.org.openscore/care-homes"
```

### Updating the listing later

Raise `version` in `server.json` (e.g. `1.0.1`), then log in and publish again (step 4). The TXT record stays in place; leave it there.

## Community directories

Submit through each site's "add / submit server" page, using:

- Server URL: `https://map.openscore.org.uk/mcp` (Streamable HTTP, no authentication)
- Name: OpenScore care homes
- Description: the `description` in `server.json`
- Website / docs: `https://map.openscore.org.uk/agents`
- Repository: this repository
- Contact: agents@openscore.org.uk

Directories:

- [Smithery](https://smithery.ai)
- [mcp.so](https://mcp.so)
- [PulseMCP](https://www.pulsemcp.com) (also picks up servers from the official registry)
