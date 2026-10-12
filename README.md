# Spanish Forge Phrases Data

The Spanish vocabulary data used by [Spanish Forge](https://spanishforge.com):
the phrase list learners study, the inflection tables behind its conjugation
and agreement grids, and the lemma table it uses to match words in a text.

It is published here because it is derived from share-alike sources. The phrase
list and inflection tables are licensed
[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The lemma
table is licensed under the
[Open Database License v1.0](https://opendatacommons.org/licenses/odbl/1-0/).

## The Data

| Path | What it is | Licence |
|---|---|---|
| `releases/phrases.json.zip` | The 4,700 headwords in Spanish Forge, A1–B2, with English glosses, CEFR levels and inflected forms | CC BY-SA 4.0 |
| `forms-table/` | The full inflection table: conjugations, noun plurals and adjective agreement for about 51,500 lemmas | CC BY-SA 4.0 |
| `lemma-table/` | Spanish form → lemma lookup, about 436,000 forms | ODbL 1.0 |

The sentences, stories, audio, images and tutor conversations in Spanish Forge
are original works. They are not adaptations of this data and are not
published here.

## `releases/phrases.json.zip`

The zip holds four files:

- `phrases.json` — the headwords, as one JSON array sorted by frequency rank
  (unranked headwords last).
- `attribution.json` — sources, licences, retrieval dates and SHA-256 hashes of
  the upstream files.
- `statistics.json` — counts by level, part of speech, gender and form source.
- `LICENSE` — the licence and required attribution.

| Level | Headwords |
|---|---|
| A1 | 700 |
| A2 | 1,000 |
| B1 | 2,000 |
| B2 | 1,000 |

### Record format

```json
{
  "spanishText": "coche",
  "englishText": "car",
  "englishVariants": [],
  "cefrLevel": 1,
  "cefr": "A1",
  "partOfSpeech": "noun",
  "gender": "m",
  "frequencyRank": 324,
  "regionalVariants": [{ "text": "carro", "region": "latam" }],
  "forms": [
    { "text": "coche", "slots": ["sg"], "isHeadword": true, "source": "wiktionary" },
    { "text": "coches", "slots": ["pl"], "isHeadword": false, "source": "wiktionary" }
  ]
}
```

| Field | Meaning |
|---|---|
| `spanishText` | The headword, as its lemma: the infinitive for verbs, the singular for nouns, the masculine singular for adjectives. Never prefixed with an article. |
| `englishText` | The primary English gloss. |
| `englishVariants` | Further English glosses. |
| `cefrLevel` / `cefr` | CEFR level as an integer 1–4 and as a label `A1`–`B2`. |
| `partOfSpeech` | `noun`, `verb`, `adjective`, `adverb`, `pronoun`, `determiner`, `preposition`, `conjunction`, `numeral`, `interjection` or `phrase`. |
| `gender` | Nouns only: `m`, `f` or `mf` (either, e.g. `estudiante`). `null` for every other part of speech. |
| `frequencyRank` | Rank in the OpenSubtitles frequency list, 1 = most frequent, with the headword's inflected forms counted together. Ranks come from the full list, so they have gaps here. `null` for the 62 headwords added to fill curriculum gaps that are not in the frequency list. |
| `regionalVariants` | Words used instead in a region: `region` is `latam` or `spain`. |
| `forms` | Inflected forms; see below. Empty for parts of speech that do not inflect. |

### Forms

Each entry in `forms` is one **spelling**. A spelling that fills more than one
slot appears once and lists every slot (`hablamos` is both present and
preterite, `["ind.pres.1pl", "ind.pret.1pl"]`). The headword is a form too,
marked `isHeadword`.

Slot ids:

- **Verbs** (56 slots) — `inf`, `ger`, `ptcp`; then `mood.tense.person` where
  mood is `ind` (tenses `pres`, `impf`, `pret`, `fut`, `cond`) or `sub` (tenses
  `pres`, `impf-ra`, `impf-se`), and person is `1sg`, `2sg`, `3sg`, `1pl`,
  `2pl` (*vosotros*), `3pl`; and the affirmative imperatives `imp.2sg`,
  `imp.3sg`, `imp.1pl`, `imp.2pl`, `imp.3pl`. Negative imperatives are the
  present subjunctive, and compound tenses are *haber* + participle, so neither
  is stored.
- **Nouns** — `sg`, `pl`.
- **Adjectives** — `m.sg`, `f.sg`, `m.pl`, `f.pl`. An invariable adjective has
  one spelling carrying both singular slots and one carrying both plurals.

`source` is where the form came from:

- `wiktionary` — English Wiktionary's inflection table for the word;
- `rule` — derived by rule, mostly pronominal verbs (`acostarse` from
  `acostar`'s table) plus a few regular plurals and agreements Wiktionary
  lacked;
- `llm` — a gap Wiktionary and the rules left, filled with the help of a large
  language model and accepted only after deterministic spelling checks and a
  second model's review (55 forms).

## `forms-table/`

`forms-table.txt.gz` is the inflection table the phrase forms are drawn from,
for every lemma that cleared the inclusion filter rather than only the 4,700
headwords. Spanish Forge uses it to show forms for words learners add
themselves.

UTF-8, gzipped, tab-separated. Lines starting with `#` are comments. One line
per surface form:

```
lemma<TAB>pos<TAB>text<TAB>slot[,slot...]<TAB>source
```

`pos` is `verb`, `noun` or `adjective`; slots are the ids above, in grid order;
`source` is `wiktionary` or `rule`. Sorted by lemma, then part of speech, then
grid position. `forms-table.meta.json` records the source file's URL, retrieval
date and SHA-256, and the lemma and row counts.

A lemma was included when it is in the lemma table, is a Spanish Forge
headword, or is the `-se` pronominal form of such a verb. Proper nouns and
multi-word entries were left out.

## `lemma-table/`

See [`lemma-table/README.md`](lemma-table/README.md) for the format and the
changes made to the source, which is what the ODbL asks a derivative database
to document.

## Sources and attribution

> Frequency list derived from hermitdave/FrequencyWords (CC BY-SA 4.0), built
> from the OpenSubtitles 2018 corpus via OPUS

- [hermitdave/FrequencyWords](https://github.com/hermitdave/FrequencyWords),
  `content/2018/es/es_50k.txt`, CC BY-SA 4.0. Built from
  [OpenSubtitles 2018](https://opus.nlpl.eu/OpenSubtitles.php) via OPUS.
  Provides the headword list and `frequencyRank`.

> Inflection tables derived from English Wiktionary (CC BY-SA 4.0) via
> wiktextract / kaikki.org

- English Wiktionary, Spanish entries, as extracted by
  [wiktextract](https://github.com/tatuylonen/wiktextract) (Tatu Ylonen) and
  published by [kaikki.org](https://kaikki.org). Wiktionary text is
  dual-licensed CC BY-SA 4.0 and GFDL
  ([Wiktionary:Copyrights](https://en.wiktionary.org/wiki/Wiktionary:Copyrights));
  the derived tables are published under CC BY-SA 4.0.

> michmech/lemmatization-lists (Michal Boleslav Měchura)

- [michmech/lemmatization-lists](https://github.com/michmech/lemmatization-lists),
  `lemmatization-es.txt`, ODbL 1.0. Used to fold the frequency list's word
  forms into headwords; the derived table is in `lemma-table/`.

### What Spanish Forge added

The English glosses, CEFR levels, parts of speech, genders and regional
variants were assigned by Spanish Forge with the help of large language models,
each graded by a model from a different vendor and accepted only above a score
threshold. They are not part of any upstream source. Neither is the mapping of
Wiktionary's form tags onto the slot ids above, or any form whose `source` is
`rule` or `llm`.

Levels were calibrated against, but not taken from, the Plan Curricular del
Instituto Cervantes and the Kelly project's Spanish list; no data from either
is included here.

## Licence

Everything except `lemma-table/` is licensed under CC BY-SA 4.0 (see
[`LICENSE`](LICENSE)). `lemma-table/` is licensed under ODbL 1.0 (see
[`lemma-table/LICENSE`](lemma-table/LICENSE)).

If you redistribute this data, or something you build from it, keep the
attribution above and share it under the same licence.

Questions or corrections: [hello@spanishforge.com](mailto:hello@spanishforge.com).
