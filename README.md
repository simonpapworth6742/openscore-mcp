# OpenScore care homes MCP server

Find and compare UK care homes from an AI assistant, using the same data as the [OpenScore map](https://map.openscore.org.uk/).

- **MCP server (Streamable HTTP):** `https://map.openscore.org.uk/mcp`
- **REST API (OpenAPI 3.1):** `https://map.openscore.org.uk/.well-known/openapi.json`
- **Guide for language models:** [`llms.txt`](https://map.openscore.org.uk/llms.txt) and [`llms-full.txt`](https://map.openscore.org.uk/llms-full.txt)
- **Developer page:** [map.openscore.org.uk/agents](https://map.openscore.org.uk/agents)

Free, read-only, and no key or sign-up needed.

## What the OpenScore is

The OpenScore is a score from 0 to 10 that combines publicly available and provider-supplied information to make care homes easier to compare. Higher is better. It is not a regulator rating (CQC, Care Inspectorate, CIW or RQIA), which is given separately. A missing score means the home has no published OpenScore; it never means zero.

## Connect

This is a hosted (remote) server: there is nothing to install.

**Claude:** Settings › Connectors › Add custom connector, then enter `https://map.openscore.org.uk/mcp`.

**ChatGPT:** Settings › Connectors (developer mode) › Create, then enter `https://map.openscore.org.uk/mcp`.

**Cursor** (`.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "openscore": { "url": "https://map.openscore.org.uk/mcp" }
  }
}
```

**VS Code** (`.vscode/mcp.json`):

```json
{
  "servers": {
    "openscore": { "type": "http", "url": "https://map.openscore.org.uk/mcp" }
  }
}
```

**Assistants that can only open web pages** can search with a GET link instead. The link format and accepted answers are in [`llms.txt`](https://map.openscore.org.uk/llms.txt).

## Tools

| Tool | What it does |
|---|---|
| `get_search_guidance` | The questions to ask, why each matters, and the accepted answers |
| `search_care_homes` | Care homes near a UK postcode or town; OpenScore chooses the search radius |
| `search_care_homes_by_area` | Every matching home in a county, region or local authority, paged |
| `find_locations` | Turns a place name into directory areas |
| `get_care_home` | Full details for one care home |
| `compare_areas` | League Table figures for areas |
| `find_events` | Upcoming care home events near a place |
| `create_shortlist_link` | A link that opens the chosen homes on the OpenScore map |

There are also two prompts (`care_home_search_intake` and `questions_for_a_visit`) and reference resources on the questions, the OpenScore, funding routes and the League Table method.

## Ask the care seeker first

Before searching, the server expects the assistant to ask who is asking, where to look, how soon a place is needed, the current situation, the type of care, funding, and what matters most and least. A search without these answers returns an error saying which question to ask next. "Not sure" is always an accepted answer; assistants must never guess.

## Rules for showing results

- Show "Source: OpenScore (openscore.org.uk)" with any results.
- Link to each home's details or map page when you mention it.
- Call the OpenScore the OpenScore; never describe it as a regulator rating.
- Distances are straight-line: say "about 2.3 miles away in a straight line".
- OpenScore is information to help shortlist care homes, not advice or a recommendation. Always visit, and check with the home and the regulator, before deciding.

## Limits and privacy

- 60 requests per minute and 1,000 per day per IP address.
- Do not send names, dates of birth, NHS or CHI numbers, addresses (other than a search postcode or town), diagnoses or medical details.
- No email address is accepted anywhere in the interface.
- Free-text answers (interests and other preferences) are used within the request and never logged or stored.
- Full terms: [map.openscore.org.uk/agents#terms](https://map.openscore.org.uk/agents#terms).

## Contact

[agents@openscore.org.uk](mailto:agents@openscore.org.uk)

This repository holds the public listing and documentation only. The server itself is run by OpenScore and its source code is not public.
