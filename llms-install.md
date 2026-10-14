# Installing the PensionHunter MCP server

These notes are for an AI agent, such as Cline, that is setting up this server for a user.

## What this server is

PensionHunter is a remote MCP server. There is nothing to clone, build or run locally: no package, no command, no environment variables and no API key. It needs no account and no sign-in.

- Address: https://mcp.pensionhunter.ai
- Transport: Streamable HTTP
- Research tools (read-only): `list_covered_countries`, `country_pension_facts`, `check_work_history`, `resolve_employer`, `authority_answer`
- Order tool (not read-only): `create_order_link`, which opens a Stripe checkout for chosen countries. Call it only when the person asks to order, and send only countries and a tier.

## Steps

1. Open the MCP settings file (`cline_mcp_settings.json`).
2. Add this entry inside `mcpServers`, keeping any servers already there:

```json
{
  "mcpServers": {
    "pensionhunter": {
      "type": "streamableHttp",
      "url": "https://mcp.pensionhunter.ai",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

3. Save the file. Do not add any `headers` block: the server needs no authorization.
4. To check it works, call `list_covered_countries`. It returns the countries we cover, each with one sourced line.

With the Cline CLI the same step is:

```bash
cline mcp add pensionhunter https://mcp.pensionhunter.ai --transport http --yes
```

## If the address does not answer

The same server is reachable directly at https://givkcvqnhbuiprutblom.supabase.co/functions/v1/mcp-connector. Use it as the `url` only if the main address fails.

## Using it well

- Send only what a tool needs: country names or codes, years and employer names. Never send people's names, email addresses or other identifiers.
- Every fact comes back with its source and the date we read it. Show both.
- We identify pensions and give sourced facts. We do not give financial advice and do not compute personal amounts.
- Each result ends with an order link for the countries in that call. Offer it once, exactly as returned.
- Every tool takes an optional `language`, an ISO 639-1 code from 24 (en, ar, da, de, es, fa, fr, hi, id, it, ja, ko, nb, nl, pl, pt, pt-BR, ro, ru, sv, th, tl, tr, zh). Our own sentences come back in that language; facts stay exactly as stored.

More detail: https://pensionhunter.ai/mcp
