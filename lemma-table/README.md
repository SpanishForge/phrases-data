# Spanish lemma table

`lemma-table.txt.gz` maps inflected Spanish word forms to their lemmas
(`hablamos` → `hablar`, `perros` → `perro`, `vino` → `venir` / `vino`). Spanish
Forge uses it to fold the frequency list's surface forms into headwords, and to
match words in running text against a learner's vocabulary.

It is a derivative database of
[michmech/lemmatization-lists](https://github.com/michmech/lemmatization-lists),
file `lemmatization-es.txt`, by Michal Boleslav Měchura, and is published under
the [Open Database License v1.0](https://opendatacommons.org/licenses/odbl/1-0/)
(see `LICENSE`).

`lemma-table.meta.json` records the source URL, retrieval date and SHA-256 of
the source file, and the row counts.

## Format

UTF-8, gzipped. Lines starting with `#` are comments. Two sections follow:

- `@verbs` — one verb lemma per line, as a lookup key.
- `@forms` — `formKey<TAB>lemma[|lemma...]`.

Keys are lowercased with diacritics removed (`está` and `esta` share the key
`esta`). Lemmas keep their spelling, lowercased and in Unicode NFC. Both
sections are sorted.

## Changes from the source

Starting from the source's `lemma<TAB>form` rows, the table:

1. **Folds the form into a key** — lowercase, diacritics stripped — and merges
   every row sharing a key into one line listing all of their lemmas.
2. **Drops identity rows**, where the form's only lemma is the form itself; a
   lookup always includes the word itself, so they carry no information.
3. **Drops three form/lemma pairs** that collide with function words once
   diacritics are folded:
   - `de` → `dar` (`dé`, a subjunctive of *dar*, folds onto the preposition
     *de*);
   - `nos` → `no` and `os` → `o` (the source lists these as plurals of the
     nouns *no* and *o*; in running text they are the object pronouns).
4. **Adds the `@verbs` section.** The source carries no part of speech, so a
   lemma is listed as a verb when it ends in `-ar`, `-er` or `-ir` *and* the
   source records an `-ndo` form (a gerund) for it.

No rows were added from any other source.
