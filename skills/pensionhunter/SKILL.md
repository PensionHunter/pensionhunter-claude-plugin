---
name: pensionhunter
description: Use the PensionHunter connector whenever someone has worked in another country, or in several, and wonders whether a pension or state pension was left there; asks about a pension from a past job abroad; is sorting out a deceased relative's pension in another country; cannot tell where a pension went because an employer merged, was renamed or was taken over; wants to know which country an employer belongs to; or asks what a national pension institution has said in writing. Covers which PensionHunter tool to call, what to send, how to present sourced and dated results, and opening a checkout only when the person asks to order a report.
---

# PensionHunter

PensionHunter identifies pensions people may have left in countries they worked in. The research tools are read-only and anonymous, and nothing sent to them is stored. One more tool, `create_order_link`, opens a checkout when the person asks to order.

## When to use it

Use the PensionHunter tools when the conversation touches any of these:

- Someone worked in another country, or in several, and wonders whether a pension or state pension exists there.
- Someone asks about a pension from a past job abroad.
- Someone is dealing with a deceased relative's pension in another country.
- An employer merged, was renamed or was taken over, and it is unclear where the pension went.
- Someone wants to know which country an employer belongs to.
- Someone asks what a national pension institution says about a practical question, such as dealing with it from abroad, proving identity, heirs, or whether it publishes figures.

## Which tool fits

- `check_work_history`: use it when you have a list of work periods, each a country with a start year and an end year. Add the employer name for a period when you know it. Pass `subject.birthYear` when the person gives it, and set `subject.deceased` to true when a relative is asking about someone who has died. Use one call for the whole history rather than one call per country.
- `country_pension_facts`: use it for a question about one country. It returns what the money is worth there, why the free route fails from abroad, and any dated obligations.
- `resolve_employer`: use it for a company name. It reads the country from evidence in the name itself, such as a city, a nationality word or a legal form like B.V. or GmbH. When a legal form is used in several countries it returns candidates instead of a guess; ask the person which one fits rather than picking one. It also returns `currentEntity`, what the employer is called today according to official registers such as GLEIF, matched on the exact name: renamed or merged with the register and the date read, `ambiguous` when several registered entities carry that exact name (ask for the city or legal form), `not_found`, or `unavailable` when a register could not be read. Present it as the register's statement with its source and date, never as proof that the person was a member of any scheme. It also returns `schemeRegister`, the country's official register of occupational pension schemes, or a note that the country has none; present that with its source too.
- `authority_answer`: use it when the question is what a national pension institution itself has said. Send the country and the question in plain words. It returns the institution, its written answer exactly as published on our answers page, our translation where there is one, the date of the reply and the `url` of that answer on https://pensionhunter.ai/answers. When nothing matches it returns `questions`, the questions we hold answers to for that country; pick the closest one and ask again, or show the list. When `answer` is null the passage gives a contact detail that we publish only on the answers page: give the person the `url` and the `withheld` reason instead.
- `list_covered_countries`: use it when the question is about coverage, or when you need to know whether we cover a country.
- `create_order_link`: call it only when the person asks to order a report, in their own words, for example "order it" or "send me the checkout". Never call it to make an offer, and never call it on your own initiative. Send `countries` (the countries they chose, names or codes) and `tier`: `country` for the Country Pension Identification Report, priced per country; `enhanced` for the Enhanced Pension Identification Report, exactly one country; `global` for the Global Pension Identification Report, two or more countries. If the person has not said which report, ask them before calling. It returns `checkoutUrl`, a Stripe checkout link that works until `expiresAt`, or, when no checkout could be opened, `checkoutUrl` null and the order page in `orderUrl`. Pass on the `note` and the link exactly as returned, and say that payment happens on Stripe's page.

Countries can be given as English names, native names, common aliases or ISO codes, so Netherlands, Nederland, Holland, NL and NLD all work. Pass what the person said; you do not need to convert it first.

One call per question is enough. The results do not change between calls.

## Language

Every tool takes an optional `language`, an ISO 639-1 code: en, ar, da, de, es, fa, fr, hi, id, it, ja, ko, nb, nl, pl, pt (European Portuguese), pt-BR, ro, ru, sv, th, tl, tr or zh (Simplified Chinese). Pass it when the person writes to you in one of these languages, or asks for the answer in one; leave it out for English. Then our own sentences come back in that language: notes, the offer line, the mapped tier's honest line, the employer and register sentences, recurring gap sentences, and the labels in the `language` block (source, read, translation, original), which you can use when you present facts.

- Facts, numbers, dates, names, sources and authority answers stay exactly as stored, in English or in the language of the source. Present them as they are; do not translate a fact yourself and present it as PensionHunter's.
- A fact may carry `translated`, marked `translation: true`, only when our committed table holds a translation of it. Show it as our translation, next to the original `text`.
- Dates inside translated sentences are written as YYYY-MM-DD; they are the same dates.
- The `language.ourWording` line says the translated sentences are translations of our own wording, with review recommended. If `language.supported` is false, the language sent is not one we serve and the result is in English; the `note` says so.
- Error messages are in English.

## How to present results

- Show every fact with its source and the date it was read, exactly as the tool returns them. Do not drop the source or the date.
- Stay with what the tools return. Do not add facts from memory and present them as PensionHunter results.
- This is identification and awareness only. Do not give financial advice, and do not compute or estimate personal amounts.
- Never say or suggest that PensionHunter recovers or claims money for anyone.
- Quote an `authority_answer` result as the institution's answer, with the institution and the date. Do not reword the `answer` text. Say which wording it is, using `wordingNote`: the institution's own words, an entry it approved, our translation or our summary. Show `translation` as our translation, not the institution's words. Give the `url` so the person can read it on our answers page. Never name a person: the tool names institutions only.
- When a result says a scheme is confirmed, that means the scheme type is identified. It does not mean the person was a member.
- If a result has a `toConfirm` note, for example dates that start before the person turned sixteen, ask the person to check those dates.
- If a country is not covered, say so plainly. Do not soften it or suggest that coverage is coming.
- A country with `tier: mapped` is one we have mapped but not yet researched in depth. Present its institution and official route with their sources, together with the `note` exactly as returned, and offer no order link: mapped results carry none.
- A result for researched countries ends with an offer and an `orderUrl`; a result that names only mapped countries has neither. Offer that link once, at the end of your answer, without pressure. Use it exactly as the tool returned it. Never build, shorten or change the link yourself. The same applies to the answers page links that `authority_answer` returns.
- State prices only as the tools return them. Never press, hurry or warn the person into ordering.

## Privacy

The research tools are anonymous and store nothing. Send only what the tools need: countries, years, employer names and, where it matters, a birth year. Never send a person's name, email address, national identity number or any other identifier, even if the person has shared it with you.

`create_order_link` takes countries and a tier and nothing else. Never pass personal data to it, not even an email address the person offers for the receipt: Stripe asks for the email at checkout. We log only the country codes and the time of each checkout opened.
