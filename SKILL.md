---
name: asd-ste100
description: "Rewrites dense or ambiguous English into ASD-STE100 Simplified Technical English for tool descriptions, error messages, system prompts, SKILL.md files, and technical docs. Triggers: disambiguate, STE100 rewrite, plain-language rewrite, rewrite so an agent cannot misread this, спростити англійський текст за STE100. Not for creative writing or client correspondence."
license: MIT
metadata:
  version: 1.1.5
  upstream_version: 0.4.0
  standard: ASD-STE100 Issue 9 (2025-01-15)
---

# Simplified Technical English (ASD-STE100) v1.1.5

ASD-STE100 is a controlled natural language and international standard. ASD (Aerospace, Security and Defence Industries Association of Europe, Brussels, Belgium) owns the standard. The ASD Simplified Technical English Maintenance Group (STEMG) maintains it. The standard removes the two main causes of misreading: words with more than one meaning, and sentences with more than one possible structure.

Official STEMG domains include **Computer Science and Information Technology: Optimization of human-machine interfaces (HMI)** alongside aerospace, industry, and academia. This skill applies STE100 discipline so an AI agent or downstream parser can read an English string without a human in the loop.

## Language Rule (Gemini Adaptation)

ASD-STE100 applies strictly to **English** source text. Even when the user writes the request in Ukrainian, keep the rewritten STE output in **English**. A Ukrainian request looks like this: "перепиши цей системний промпт за STE100". When the user requests a rule table or explanation in Ukrainian, write the table headers and notes in Ukrainian. Keep the `Original` and `Simplified` columns in English.

## When to Use This Skill

- An agent instruction, system prompt, error message, status report, or tool description reads as dense, hedged, or ambiguous.
- Text will be parsed by another agent, a `SKILL.md` router, a translation pipeline, or a non-native English reader where misreading has a real cost.
- You want a before/after rule table showing each rule violation and its fix.

Do not use this skill for creative writing or client-facing correspondence where tone, warmth, or persuasion is the goal (use `humanizer-pro` for human-natural prose). If the user asks you to rewrite such text, refuse with one sentence and stop (see Boundaries). Marketing-flavored technical prose, such as a product description in a README or a PR description, is in scope for STE-flavored mode. Rewrite it and remove the marketing adjectives.

## Three Modes

Select one mode before rewriting. Infer the mode from the text type if the user does not specify one. Keep the mode choice internal unless the user asks for the rule table.

1. **Strict** - Procedures, error messages, safety text, and operational instructions where a wrong reading has a cost. Enforce all structural rules, hard word caps (<=20 words per instruction sentence, <=25 words per descriptive sentence), and consistent word choice.
2. **Agent/Skill Spec** - System prompts, `SKILL.md` files, YAML `description` fields, `function_declarations`, and inter-agent protocols (aligned with STEMG HMI and FAIR data interoperability). Enforce <=20 words per sentence. Replace ambiguous leading pronouns (`it`, `this`, `they`) with explicit nouns (`The agent`, `The linter`, `The user`). Structure conditionals as `If <condition>, <actor> <verb> <object>.`. Preserve exact backticked identifiers (`` `SKILL.md` ``, `` `--json` ``). Copy every backticked identifier from the source into the rewrite unchanged. After the rewrite, list every backticked identifier from the source. Check that each one appears in the rewrite. Start every conditional sentence with `If` or `Only when`. A condition that appears after a comma is a violation.
3. **STE-flavored** - READMEs, PR descriptions, changelogs, and explanatory prose. Enforce all structural rules: word caps, active voice, simple tenses, no phrasal verbs, no semicolons, no nominalizations, and no marketing adjectives. Rewrite marketing-flavored technical prose in this mode. Treat the one-word-one-meaning restriction as advisory so prose stays readable.

## Core Structural Rules

- **Active voice:** Name the actor (`The agent deletes the file.`). Use passive voice only in descriptive text when the actor is unknown or irrelevant.
- **No phrasal verbs (Rule 9.3):** Use single plain verbs (`Remove the panel.`, `Start the job.`), not two-word verbs (`Take off`, `Spin up`, `Reach out`, `Kick off`).
- **One instruction per sentence:** Split compound commands into separate sentences.
- **Sentence length caps:** Maximum 20 words for instructions and agent specs. Maximum 25 words for descriptions. Count whitespace-separated tokens containing at least one letter or digit. Exclude backticked inline code, list markers, and standalone punctuation.
- **No semicolons (Rule 8.1):** Never use the semicolon mark. Split into separate sentences. Other standard punctuation marks remain permitted.
- **Noun clusters:** Maximum 3 stacked nouns (`fuel pump valve`). Rewrite 4+ noun stacks with prepositions or relative clauses.
- **No ellipsis:** Keep subject, verb, and article explicit. Leave nothing to inference.
- **Preserve modality:** Keep hedges (`may`, `might`, `could`, `may have failed`) intact. Confidence is content. Never upgrade a hedge into an unverified fact.
- **Paragraph limits:** One topic per paragraph, maximum 6 sentences. Use bulleted or numbered lists for 3 or more steps or conditions.

## Lexical Discipline (Direction of Travel)

This skill applies the underlying STE principle (pick the plainest common word and reuse it consistently) rather than reproducing ASD's copyrighted ~900-word approved dictionary verbatim:

- **One word, one meaning:** Pick one verb per action and reuse it throughout the document (always `check`, never rotate `check` / `verify` / `confirm`).
- **One part of speech per word:** Prefer noun forms for objects (`Apply oil to the valve.`) instead of zero-derived verbs (`Oil the valve.`).
- **Verb, not noun (Rule 3.7):** Use direct verbs (`Analyze the log.`), not nominalizations (`Perform an analysis of the log.`).
- **Simple tenses:** Use infinitive, imperative, simple present, simple past, or simple future. Replace present perfect (`We have received`) with simple past (`We received`), except when current relevance or a modal hedge (`may have failed`) carries essential meaning.
- **Domain glossary:** Keep required technical identifiers and define unfamiliar domain terms once.

## Six-Habit Scan Checklist

Scan every input for these six patterns before rewriting:

1. **Synonym rotation:** Multiple nouns or verbs naming the same entity or action.
2. **Hedge stacking:** Piling qualifiers (`it is important to note that this may potentially help`) that obscure the claim.
3. **Nominalization:** Actions frozen into nouns (`perform an analysis of`, `provides assistance to`).
4. **Marketing adjectives:** Unmeasured claims (`seamless`, `robust`, `powerful`, `cutting-edge`, `effortless`, `blazing-fast`).
5. **Run-on sentences:** Multiple clauses joined by semicolons, em-dashes, or chained conjunctions.
6. **Soft phrasal verbs:** Idiomatic verb-preposition pairs (`spin up`, `reach out`, `dive into`, `kick off`, `circle back`, `touch base`).

## Execution Workflow

1. **Select mode:** Choose `Strict`, `Agent/Skill Spec`, or `STE-flavored`.
2. **Read for meaning:** Identify every fact, condition, scope qualifier, and hedge before editing.
3. **Scan and rewrite:** Fix every structural and lexical violation without dropping facts or inventing causes, frequencies, or mechanisms.
4. **Dual-Pass Word Budget Check:** Count the words in every rewritten sentence and list the counts on the Word budget line after the text, for example `Word budget: 13, 7, 11, 9 words (cap 20).`. Count whitespace-separated tokens that contain at least one letter or digit. Exclude backticked inline code, list markers, and standalone punctuation. If a sentence exceeds the 20-word cap (instructions and agent specs) or the 25-word cap (descriptions), split it. Keep a sentence whole only when splitting loses precision, and add a line that starts with `Kept as-is:` and names the precision preserved. Never declare a sentence compliant when its count exceeds the cap.
5. **Check compliance:** If the input already complies, state that it complies instead of forcing unnecessary edits. State the word counts that prove compliance.

## Output Format

- **Default output:** Return the rewritten English text alone. Then add one line after the text with the word count of every sentence. Example: `Word budget: 13, 7, 11, 9 words (cap 20).` Add no preamble, mode label, or other commentary.
- **Exception note:** If step 4 kept a long sentence on purpose to protect precision, add one line after the text. Start the line with `Kept as-is:`. Name the phrase and the precision preserved.
- **On request (`show the diff`, `which rules did it break`, `покажи різницю`):** Output a markdown table with columns `Rule violated | Original | Simplified`, followed by `Mode: <mode>. <N> violations found.` and any `Kept as-is:` note.

## Boundaries and Legal Notice

- **Trademark and affiliation:** Simplified Technical English and ASD-STE100 are a Copyright and European Union Trade Mark (No. 017966390) of ASD, Brussels, Belgium. This skill is an independent clarity tool inspired by ASD-STE100 Issue 9 (January 15, 2025). It is not endorsed, certified, or affiliated with ASD or ASD STEMG. For certified aerospace/defense documentation, obtain the official standard from `https://www.asd-ste100.org/`.
- **Ecosystem boundaries:** Do not rewrite creative writing or client emails (route to `humanizer-pro`). Never invent facts or claims absent from the source (aligned with `no-invention`).
- **Out-of-scope input:** If the input is creative writing or client-facing correspondence, do not rewrite it. Reply with one sentence in the language of the request, then stop. English form: "This input is out of scope for STE. Use humanizer-pro for human-natural prose."

## Bundled Resources

- `references/writing-rules.md` - Summary of the 53 ASD-STE100 Issue 9 rules, evolution timeline, and official STEMG sources.
- `references/gemini-integration.md` - Guide for applying `Agent/Skill Spec` mode to Gemini `SKILL.md` files, tool schemas, and system prompts.
- `examples/before-after.md` - Worked before/after transformations for STE rules and AI agent outputs.
