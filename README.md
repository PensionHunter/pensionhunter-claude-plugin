# PensionHunter plugin for Claude

PensionHunter identifies pensions people may have left in countries they worked in. This plugin brings our public connector into Claude, together with a skill that tells Claude when to use it and how to present what it returns.

## What the plugin contains

- **The PensionHunter connector.** The file `.mcp.json` adds one remote MCP server at https://mcp.pensionhunter.ai over HTTP. It serves these read-only research tools:
  - `list_covered_countries` lists the countries we cover, each with one sourced line on what exists there.
  - `country_pension_facts` returns the stored facts for one country: what the money is worth, why the free route fails from abroad, and dated obligations. For a country we have mapped but not yet researched in depth, it returns the paying institution and its official route instead, marked `tier: mapped`, with no offer.
  - `check_work_history` takes a list of work periods, each a country and years, and says for each country whether a pension is likely to exist and why.
  - `resolve_employer` reads the country an employer name implies, from evidence in the name itself. It also says what the employer is called today, read by exact name from official registers such as the Global LEI Index (GLEIF), with the register and the date we read it, and it names the employer's country register of occupational pension schemes or says that the country has none.
  - `authority_answer` takes a country and a question in plain words and returns what a national pension institution told us in writing, as published on our answers page: the institution, its answer exactly as published, our translation where there is one, the date of the reply and the link to that answer on https://pensionhunter.ai/answers. When nothing matches it lists the questions we hold answers to for that country. When we have written to the country and hold no answer yet, it says so with the date we asked. It names institutions only, never a person.

  And one tool that is not read-only:
  - `create_order_link` opens a secure Stripe checkout for one of our reports, for the countries the person chose, and returns its link, which works for about 24 hours. Claude calls it only when the person asks to order. It sends only the country codes and the report tier; Stripe asks for the email address at checkout. If a checkout cannot be opened, it returns our order page with the same choices filled in.
- **Answers in 24 languages.** Every tool takes an optional `language` (an ISO 639-1 code such as fr, de, ja or pt-BR). Our own sentences then come back in that language from committed tables, marked as translations of our own wording with review recommended. Facts, numbers, dates and authority answers stay exactly as stored, in English or the language of the source.
- **The `pensionhunter` skill.** It tells Claude when the connector fits, which tool to call, what to send, and how to present results: every fact with its source and read date, no financial advice, no personal amounts, the order link offered once, exactly as the tool returns it, and `create_order_link` only when the person asks to order.

The plugin runs no code on your computer. It has no hooks, no scripts and no local server. The only thing it connects to is https://mcp.pensionhunter.ai.

## What is sent, and what is stored

When Claude calls a tool, it sends the inputs that tool needs: country names or codes, years, employer names, a question in plain words for `authority_answer` and, optionally, a birth year and whether the person has died. Nothing else is sent. The tools need no account and no sign-in, and we store nothing that is sent to them. To say what an employer is called today, our server looks the employer name up in official registers, starting with the Global LEI Index; no other input goes to a third party. The skill tells Claude never to send names, email addresses or other identifiers.

Every result ends with a link to our order page for the countries in that result. The link carries the country codes only, never any personal detail. Results from `authority_answer` also link each answer on our answers page.

When the person asks to order, `create_order_link` sends our server the country codes and the report tier, nothing else. We open a Stripe checkout session for them and log the country codes and the time, nothing more. The person enters their email address and pays on Stripe's page; payment always stays their own action.

## Install

**In claude.ai, the Claude desktop app or Cowork.** Open Customize, then Plugins, find PensionHunter in Discover, and select Add. Then open the plugin's Connectors tab and add the PensionHunter connector. It needs no sign-in.

**In Claude Code.** A plugin added on claude.ai syncs to Claude Code when you sign in with the same account; run `/reload-plugins` to load it. If you have a copy of this plugin folder, you can also load it for one session:

```bash
claude --plugin-dir ./claude-plugin
```

**The connector on its own.** You can also add only the connector, without the skill:

```bash
claude mcp add --transport http pensionhunter https://mcp.pensionhunter.ai
```

## Good to know

We identify pensions and raise awareness of them. We do not move money or act for anyone, and we do not compute personal amounts. Not regulated by the FCA. We do not provide financial advice.

## Links

- Connector setup and details: https://pensionhunter.ai/mcp
- Privacy policy: https://pensionhunter.ai/privacy
- Terms: https://pensionhunter.ai/terms
- Contact: hello@pensionhunter.ai

The PensionHunter Team
