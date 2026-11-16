# ASD-STE100 Skill v1.1.5

By Vasyl & Mia

Simplified Technical English (ASD-STE100, Issue 9) as an AI skill for Google Gemini & Meta Muse.

This skill rewrites dense or ambiguous English into clear ASD-STE100 Simplified Technical English, so an AI agent or a reader cannot misread it.

## What is new in v1.1.5

- Three modes: Rewrite, Check, and Agent/Skill Spec — for skill descriptions, system prompts and tool texts.
- Language Rule: the rewritten text stays English, even when the request is in another language.
- Word budget: every rewrite ends with one line showing the word count of each sentence.
- Narrow refusal: creative writing and client correspondence are out of scope; marketing-flavoured technical prose is rewritten.
- Six-Habit Scan Checklist and Dual-Pass Word Budget Check.
- Python linter (scripts/ste-lint.py) checks any text against the rules.

## Install

- Gemini: install as a skill from the SKILL.md and the references in this repository (the Gemini edition, without the linter).
- Command line: run the linter with python scripts/ste-lint.py to check a text.

## Origin

Forked from danyuchn/asd-ste100-skill (v0.4.0). Built on the ASD-STE100 standard, Issue 9, January 2025. This project is not endorsed, certified, or affiliated with ASD or ASD STEMG.
