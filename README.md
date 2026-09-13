# sie-index

Structured headword index for the *Soviet Historical Encyclopedia*
(Советская историческая энциклопедия), OCR-extracted and
human-corrected.

A machine-extracted, human-reviewed index of headwords (article
titles) from the encyclopedia, with each entry's printed
volume/page/column location. Volume 1 has had a full first pass of
extraction and proofreading (a few known errors remain and are being
worked through); further volumes will be added as they're processed.

Produced with [kolonka](https://github.com/fukanchik/kolonka), an OCR
pipeline and web editor built for exactly this kind of two-column
reference-work digitization.

## Contents

```
data/
  SIE-01.csv   -- Volume 1, full data, in scan (page) order
  SIE-01.md    -- Volume 1, simplified table, alphabetically sorted --
                  browsable directly on GitHub, no download needed
```

The CSV has every field, in the order pages were scanned — meant for
scripts/analysis. The `.md` file is a simplified three-column table
(Title, Page, Printed #) sorted alphabetically like a real index, meant
for people browsing in a web browser; it renders directly on GitHub's
file view. A `*` after a printed page number there means it was
extrapolated rather than read directly from that page's own header
(see the [kolonka](https://github.com/fukanchik/kolonka) README for
how that works) — everything else in the CSV (bold-stroke score, raw
OCR text) is left out of the `.md` version to keep it readable.

CSV columns:

| column            | meaning |
|-------------------|---------|
| `page`            | scanned page number in the source djvu |
| `column`          | `left` or `right` (this encyclopedia prints two numbered columns per physical page) |
| `column_page`     | the printed page number for that column |
| `column_page_end` | for entries spanning more than one printed column/page; empty otherwise |
| `guessed`         | `1` if `column_page` was extrapolated rather than read directly from this page's own header; `0` if directly read or manually confirmed |
| `title`           | the headword text |
| `bold_ratio`      | stroke-weight score used during extraction to confirm the text is actually typeset bold (i.e. a real headword, not body text) |
| `first_line`      | OCR'd text from inside the final marked region, kept for spot-checking |

## Regenerating from source

The export script (`export.py`) lives in the
[kolonka](https://github.com/fukanchik/kolonka) repo, since it depends
on that project's database schema. Clone it, then point it at your
`sie_titles.db` with `--out` set to this repo's `data/` directory:

```bash
git clone https://github.com/fukanchik/kolonka.git
python3 kolonka/export.py /path/to/sie_titles.db --out data
# or just one volume:
python3 kolonka/export.py /path/to/sie_titles.db --volume СИЭ-01 --out data
```

## License

**This index** (the compiled list of titles, page/column numbers, and
the structure connecting them) is released under
[CC0 1.0](LICENSE) — public domain, no restrictions, no attribution
required. Use it however you like.

**The underlying encyclopedia's own copyright status is a separate
matter** that this license does not address. This repository contains
only extracted titles and their locations, not the encyclopedia's
article text — but the *Soviet Historical Encyclopedia* itself
(Moscow: Sovetskaya Entsiklopediya, 1961–1976) may still be under
copyright in some jurisdictions depending on Soviet/Russian
copyright-term rules and treaty timing. If you plan to use this index
in a way where that distinction matters to you, that's worth its own
independent legal assessment — nothing here should be read as a
determination that the source work itself is in the public domain.

## Authorship

The extraction pipeline and editor (kolonka) were written by Claude
(Anthropic); see that repository for details. This dataset itself —
the specific titles, page numbers, and corrections in `data/` — was
produced and reviewed by the human maintainer of this repository using
that tool.
