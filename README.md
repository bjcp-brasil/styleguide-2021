# BJCP 2021 Parsed Style Guidelines

This repository distributes the parsed version of the [2021 BJCP Style Guidelines](https://www.bjcp.org/bjcp-style-guidelines/) in JSON, XML and YAML formats — in English (`bjcp-2021.*`) and, since 2026, in Brazilian Portuguese (`bjcp-2021-pt-br.*`).

## Files

| File | Language | Source |
|---|---|---|
| `bjcp-2021.json` / `.xml` / `.yaml` | English | Original BJCP text |
| `bjcp-2021-pt-br.json` / `.xml` / `.yaml` | Português (Brasil) | Translation maintained at [bjcp-brasil/bjcp-2021-pt-br](https://github.com/bjcp-brasil/bjcp-2021-pt-br) |

Same schema in both languages: `styleguide.category[]` (`id`, `name`, `notes`, `subcategory[]`), each `subcategory` with `id`, `name`, `impression`, `aroma`, `appearance`, `flavor`, `mouthfeel`, `comments`, `history`, `ingredients`, `comparison`, `statistics` (`og`/`ibus`/`fg`/`srm`/`abv`, each with `min`/`max`), `examples[]`, `tags[]`.

## How the PT-BR files are generated

Unlike the English files (maintained by hand), `bjcp-2021-pt-br.*` is generated
from the `.tex` source of [bjcp-2021-pt-br](https://github.com/bjcp-brasil/bjcp-2021-pt-br)
(the same `.tex` used for that repo's PDF and website) by a script that:

1. Converts each style `.tex` to Markdown via `pandoc`.
2. Extracts each labeled field (`**Impressão Geral**:`, `**Aroma**:`, etc.)
   into the matching JSON key, tolerating known label variants found across
   the translation (e.g. "Impressão Geral" / "Impressões Gerais").
3. Parses the `OG`/`IBU`/`FG`/`SRM`/`ABV` statistics table; for open-ended
   categories without a fixed range (e.g. Fruit Beer, Experimental Beer),
   keeps the free-text note instead (`"statistics": {"notes": "..."}`),
   matching how the English file already handles similar cases (e.g. 21B
   Specialty IPA overview).
4. Splits comma-separated `Exemplos Comerciais` and `Atributos de Estilo`
   into `examples`/`tags` arrays.
5. `bjcp-2021-pt-br.xml` and `.yaml` are derived directly from the JSON
   (not hand-edited), same array-as-repeated-tag convention as the
   English XML.

The script isn't committed here (it lives with the translation source) — if
this dataset needs a refresh (new revision of `bjcp-2021-pt-br`), re-run it
and replace these three files wholesale, don't hand-edit them.

### Known gaps in the PT-BR dataset

Structure (category/subcategory `id`s, counts) matches the English file
1:1 — verified against every category and subcategory. A couple of
content-level differences, inherited from the translation source, not the
conversion:

- One style (`27A. Historical Beer: Kellerbier`) has no `tags` — the
  translation omits the "Atributos de Estilo" line for that entry.
- Historical Beer subcategories use id `27A` here (matches the actual
  BJCP subcategory code used in the source `.tex`), while the English
  file uses the bare `27` for the same entries — a deliberate choice to
  keep the more informative id rather than replicate that quirk.

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

**English files**: if you found an issue, fix the JSON version first, then
regenerate XML/YAML with a tool such as [ConvertJson](https://www.convertjson.com/json-to-xml.htm).

**PT-BR files**: fix the source `.tex` in
[bjcp-2021-pt-br](https://github.com/bjcp-brasil/bjcp-2021-pt-br) instead of
editing `bjcp-2021-pt-br.*` directly — these are generated and will be
overwritten on the next regeneration.

## License

Everything about the 2021 Style Guidelines is owned by [BJCP](http://bjcp.org).
