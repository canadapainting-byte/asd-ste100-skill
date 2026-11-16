# Gemini Integration Guide (`Agent/Skill Spec` Mode)

Use this guide when applying `asd-ste100` to Gemini skills, tool declarations, system instructions, or inter-agent prompts. This aligns with ASD STEMG's IT domain (**Optimization of human-machine interfaces**) and **FAIR** (Findable, Accessible, Interoperable, Reusable) machine-readable documentation principles.

## 1. Optimizing `SKILL.md` Frontmatter `description`

The YAML `description` is the sole trigger signal evaluated before a skill loads. Ambiguous verbs or bloated clauses cause false triggers or missed activations.

- **Target length:** 150 to 400 characters (hard platform limit: 1024 characters).
- **Structure:** `[Primary capability in active present tense]. Triggers: [exact keywords]. Not for [explicit exclusions].`
- **Rules:**
  - Start with a concrete verb (`Rewrites`, `Audits`, `Generates`, `Extracts`), never `A skill that helps you to...`.
  - Eliminate marketing adjectives (`powerful`, `seamless`, `intelligent`, `comprehensive`).
  - Keep `Not for ...` explicit so neighboring skills do not collide.

## 2. Optimizing Tool and Function Descriptions

Downstream models parse `function_declarations` without human clarification. Every ambiguous pronoun or buried condition causes wrong tool calls.

- **Sentence pattern:** `[Actor] + [Active Verb] + [Object] + [Explicit Condition].`
- **Pronoun ban:** Replace `it`, `this`, `that`, and `they` at the start of sentences with the exact noun (`The tool`, `The server`, `The file`).
- **Precondition placement:** Put `If ...` or `Only when ...` at the start of the sentence, never tacked onto the end after a comma.

### Example - Tool Parameter Description

- **Before (38 words, passive, buried condition, vague pronoun):**
  > This parameter is used to specify the target directory where files will be saved, though it should only be provided when a multi-file skill is being created and must not be combined with raw content.
- **After (3 sentences, <=14 words each, active, explicit):**
  > Specify the target directory path for a multi-file skill. Omit this parameter for single-file skills. Do not combine `skill_dir` with `content`.

## 3. Optimizing Inter-Agent Protocols and System Prompts

When an orchestrator passes instructions to a worker agent (for example, Gemini to Meta Muse or a sub-agent):

- **Split branching logic:** Write one `If <condition>, <action>.` sentence per branch. Never chain `if ... else if ... otherwise` inside one sentence.
- **Preserve uncertainty:** Keep `may`, `might`, and `could` intact when reporting diagnostic states so downstream agents do not treat hypotheses as verified facts.
- **Protect identifiers:** Keep file paths, CLI flags, JSON keys, and enum values inside single backticks so neither the rewriter nor a translation layer alters them.
