# logicgrid-puzzles

Puzzle content for the LogicGrid app. **Generated, not hand-edited** -- rebuilt
from [logicgrid-generator][gen] with `scripts/build_content_repo.py`. Edits
here are lost on the next rebuild; change the generator instead.

[gen]: https://github.com/fishhead2567/logicgrid-generator

50 puzzles, ratings 1-7, 0 with a verified narrative.
Generated 2026-11-22T00:12:37+00:00 by logicgrid 0.1.0.

## Fetch contract

The app needs no git client. Everything is plain HTTPS against the raw files:

1. `GET manifest.json`
2. Compare each entry's `sha256` with what you have cached.
3. Download only what changed, and verify the hash before replacing a cached
   file. A truncated download must never overwrite a good puzzle.
4. If any step fails, keep using the cache. The app ships an initial set in its
   assets so a first run works with no network at all.

## manifest.json

```json
{
  "schema": 1,
  "generated": "<ISO 8601 UTC>",
  "generator": "logicgrid <version>",
  "count": 50,
  "puzzles": [
    {
      "pid": "<stable content hash -- key your progress on THIS>",
      "id": "<filename stem, NOT stable across regeneration>",
      "title": "...", "theme": "...", "profile": "gentle|easy|moderate|hard|expert",
      "rating": 1, "categories": 3, "items": 4, "clues": 5,
      "puzzle": {"path": "puzzles/<id>.json", "sha256": "...", "bytes": 0},
      "narrative": {"path": "narratives/<id>.json", "sha256": "...", "bytes": 0}
    }
  ]
}
```

`narrative` is absent when a puzzle has none; fall back to the canonical clues,
which every puzzle has.

### Key progress on `pid`

`id` is a filename and moves when the set is regenerated. `pid` is a hash of
the puzzle's content, so it identifies one puzzle for good. Keying completion
on `id` means a regeneration silently marks the wrong puzzles done.

## Puzzle format

`puzzles/<id>.json` carries `schema` (the categories and their items), `clues`
(typed clue atoms), `solution`, and `text` -- the canonical prose, pre-rendered.

**Use `text`.** Rendering a clue from the atoms means re-implementing ~375 lines
of English morphology (irregular verb stems, do-support negation, article
insertion, pluralisation, number words, per-spine comparatives). `text.intro`
and `text.clues` are exactly what the reference terminal player shows, so every
client reads identically. The atoms are there for anything that needs the
logic -- a hint engine, a solver -- not for display. Every puzzle in here has been verified to
have exactly one solution, reachable without guessing unless its profile says
otherwise.

Note that the solution ships with the puzzle, so the app can check answers and
reveal them offline. Anyone reading this repo can therefore read the answers --
a deliberate trade for offline play.

## Narrative format

`narratives/<id>.json` holds `title`, `introduction` and `clues` (prose). Each
one has been read back by three independent extractors and proved to yield the
same unique solution as the canonical clues. `verdict` is `pass`, or
`pass_equivalent` where the readers agreed on the answer by differently-shaped
routes.

## Licence

MIT -- see [LICENSE](LICENSE). That covers the puzzle content here as well as
the manifest.

Logic grid puzzles of this shape are a long-standing magazine format. These are
written in the style of Penny Dell's "Logic Problems", but "Penny Dell" is
someone else's trademark and nothing here is affiliated with or endorsed by
them.
