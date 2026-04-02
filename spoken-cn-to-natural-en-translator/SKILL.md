---
name: spoken-cn-to-natural-en-translator
description: Translate colloquial spoken Chinese into natural, context-aware English by adapting phrasing, terminology, and implied meaning instead of following the Chinese wording literally.
---

# Spoken Chinese To Natural English Translator

## What It Does

- Translates colloquial Chinese into fluent, natural English that sounds locally used rather than word-for-word converted.
- Rebuilds sentence structure around English usage, tone, and implied meaning instead of preserving Chinese order mechanically.
- Normalizes technical terms, product terms, role names, and proper nouns from context when the Chinese wording is informal, imprecise, or locally misused.
- Preserves the speaker's intent, relationship tone, and practical meaning while removing translation artifacts.

## When It Should Trigger

- The user wants spoken or chat-style Chinese translated into natural English.
- The source text includes implied subjects, omitted connectors, slang, softened requests, or culturally Chinese phrasing that should be rewritten for real English usage.
- The request includes technical, workplace, product, or domain terms that may need contextual correction instead of literal translation.

## When It Should Not Trigger

- The user wants a literal, study-oriented, or line-by-line translation that mirrors the Chinese wording closely.
- The task is mainly English copywriting from scratch rather than translation from Chinese source material.
- The task is mainly long-form ESL polishing of existing English rather than Chinese-to-English translation.
- The source text is so ambiguous that multiple materially different meanings are equally likely and no grounded inference is possible.

## Expected Inputs

- Chinese source text
- Optional context such as speaker role, audience, setting, industry, product, or region
- Optional tone target such as casual, professional, friendly, direct, or customer-facing
- Any fixed terms, names, or bilingual vocabulary that must be preserved if known

## Expected Outputs

- A clean English translation that reads like something a native or locally fluent speaker would actually say
- Brief clarification notes only when ambiguity or terminology correction materially affects meaning
- Optional alternative phrasings when tone can reasonably vary and that variation matters to the request

## Workflow

### 1. Identify The Real Message

- Read for intent first, wording second.
- Recover omitted subjects, time references, and causal links that are natural in Chinese speech but must often be made explicit in English.
- Distinguish between what is said directly and what is implied socially, such as soft refusals, face-saving language, or indirect requests.

### 2. Infer The Correct Context

- Use the surrounding context to decide whether a term refers to a feature, release, environment, ticket, vendor, team, customer, or something else nearby in meaning.
- Treat Chinese shorthand and habitual misuse as signals, not as final terminology.
- Prefer the English term that a practitioner in that setting would actually use.

### 3. Rebuild For Natural English

- Rewrite into English-first syntax and rhythm.
- Replace Chinese discourse patterns with English ones, including topic-first openings, repeated subjects, and stacked modifiers.
- Choose verbs and collocations that sound native in context. For example, prefer "ship," "deploy," "roll out," or "launch" based on situation rather than translating "上线" mechanically.

### 4. Preserve Tone Without Preserving Chinese Form

- Keep the original level of politeness, urgency, confidence, and social distance.
- Convert blunt Chinese phrasing into normal English where needed, and convert overly indirect Chinese phrasing into clear but natural English where appropriate.
- Do not make the speaker sound robotic, overly formal, or culturally mismatched.

### 5. Handle Terms And Names Carefully

- Keep established product names, APIs, companies, and person names stable.
- When a Chinese term is probably not the real English label, infer the likely intended term from context and use that.
- If a term could map to more than one valid English concept, choose the strongest contextual fit and briefly note the ambiguity only when it matters.

### 6. Decide Whether To Ask Or Infer

- If the ambiguity is minor, infer and continue.
- If the ambiguity would change the business meaning, technical meaning, or speaker intent substantially, ask a short targeted question or provide the best translation with a brief note.
- Do not stop for low-value wording uncertainty.

## Translation Rules

- Optimize for meaning, idiomatic usage, and local phrasing.
- Do not preserve Chinese word order unless it already works naturally in English.
- Do not translate technical or professional terms literally when domain usage points elsewhere.
- Do not over-explain cultural subtext inside the translation itself.
- Do not add information that is not reasonably implied by the source and context.

## Output Shape

Default to this behavior:

- Give the final English directly when the meaning is clear.
- Add a one-line note only if you corrected a likely mistranslated term or resolved a meaningful ambiguity.

When the user wants options, prefer:

```md
Natural English:
[final translation]

Notes:
- [only if needed]

Alternative:
[optional variant with different tone]
```

## Questioning Strategy

- Ask only when the missing context changes the likely English meaning in a meaningful way.
- Keep questions narrow and binary where possible, such as whether a term refers to a software release or a marketing launch.
- If the request is obviously for fast translation, provide the best high-confidence version first and keep follow-up minimal.

## Handoff Expectations

- If the user later wants the translated English polished into a longer article, email, or essay, hand off to a writing-oriented skill after the translation meaning is settled.
- If the user wants terminology consistency across a product or technical domain, keep a compact term list in the response when useful so downstream work can reuse it.

## Non-Goals

- Literal textbook translation
- Full bilingual annotation unless requested
- Writing original English content unrelated to a Chinese source
- Acting as a general-purpose interpreter for every multilingual task
