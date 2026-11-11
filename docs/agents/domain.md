# Domain docs

Layout: single-context. One `GLOSSARY.md` and `docs/adr/` at the repo root.

## Before exploring, read

- `GLOSSARY.md` at the repo root, if it exists.
- `docs/adr/`: ADRs touching the area you're changing.

Missing files: proceed silently. Created lazily by `/domain-modeling`.

## Use glossary vocabulary

Name domain concepts as `GLOSSARY.md` defines them. Don't drift to synonyms it avoids. Missing term: either reconsider the wording or note the gap for `/domain-modeling`.

## Flag ADR conflicts

If output contradicts an ADR, surface it explicitly:

> Contradicts ADR-0007 (...), worth reopening because...
