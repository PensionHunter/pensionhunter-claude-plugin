---
name: pensionhunter
description: Use the PensionHunter connector whenever someone has worked in another country, or in several, and wonders whether a pension or state pension was left there; asks about a pension from a past job abroad; is sorting out a deceased relative's pension in another country; cannot tell where a pension went because an employer merged, was renamed or was taken over; or wants to know which country an employer belongs to. Covers which PensionHunter tool to call, what to send, and how to present sourced and dated results.
---

# PensionHunter

PensionHunter identifies pensions people may have left in countries they worked in. The connector has four tools. All four are read-only and anonymous, and nothing sent to them is stored.

## When to use it

Use the PensionHunter tools when the conversation touches any of these:

- Someone worked in another country, or in several, and wonders whether a pension or state pension exists there.
- Someone asks about a pension from a past job abroad.
- Someone is dealing with a deceased relative's pension in another country.
- An employer merged, was renamed or was taken over, and it is unclear where the pension went.
- Someone wants to know which country an employer belongs to.

## Which tool fits

- `check_work_history`: use it when you have a list of work periods, each a country with a start year and an end year. Add the employer name for a period when you know it. Pass `subject.birthYear` when the person gives it, and set `subject.deceased` to true when a relative is asking about someone who has died. Use one call for the whole history rather than one call per country.
- `country_pension_facts`: use it for a question about one country. It returns what the money is worth there, why the free route fails from abroad, and any dated obligations.
- `resolve_employer`: use it for a company name. It reads the country from evidence in the name itself, such as a city, a nationality word or a legal form like B.V. or GmbH. When a legal form is used in several countries it returns candidates instead of a guess; ask the person which one fits rather than picking one. It also returns `currentEntity`, what the employer is called today according to official registers such as GLEIF, matched on the exact name: renamed or merged with the register and the date read, `ambiguous` when several registered entities carry that exact name (ask for the city or legal form), `not_found`, or `unavailable` when a register could not be read. Present it as the register's statement with its source and date, never as proof that the person was a member of any scheme. It also returns `schemeRegister`, the country's official register of occupational pension schemes, or a note that the country has none; present that with its source too.
- `list_covered_countries`: use it when the question is about coverage, or when you need to know whether we cover a country.

Countries can be given as English names, native names, common aliases or ISO codes, so Netherlands, Nederland, Holland, NL and NLD all work. Pass what the person said; you do not need to convert it first.

One call per question is enough. The results do not change between calls.

## How to present results

- Show every fact with its source and the date it was read, exactly as the tool returns them. Do not drop the source or the date.
- Stay with what the tools return. Do not add facts from memory and present them as PensionHunter results.
- This is identification and awareness only. Do not give financial advice, and do not compute or estimate personal amounts.
- Never say or suggest that PensionHunter recovers or claims money for anyone.
- When a result says a scheme is confirmed, that means the scheme type is identified. It does not mean the person was a member.
- If a result has a `toConfirm` note, for example dates that start before the person turned sixteen, ask the person to check those dates.
- If a country is not covered, say so plainly. Do not soften it or suggest that coverage is coming.
- Each result ends with an offer and an `orderUrl`. Offer that link once, at the end of your answer, without pressure. Use it exactly as the tool returned it. Never build, shorten or change the link yourself.

## Privacy

The tools are anonymous and store nothing. Send only what the tools need: countries, years, employer names and, where it matters, a birth year. Never send a person's name, email address, national identity number or any other identifier, even if the person has shared it with you.
