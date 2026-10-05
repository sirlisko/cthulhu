# Call of Cthulhu campaign — project conventions

Companion site for a finished Call of Cthulhu 7e campaign (season 1, from the
United States to Berlin), built with Astro. The sections (capitoli/diario,
riassunto, investigatori, comprimari, luoghi) are cross-linked with
Obsidian-style wikilinks. Forked from the
[strahd](https://github.com/sirlisko/strahd) site; the visual theme is an
occult "Miskatonic tome".

**Language:** all site content (everything under `src/content/` and any
user-visible text in pages/components) is written in **Italian**. Code,
comments, commit messages and docs are in English. Collection names,
frontmatter keys and URL routes are Italian and must stay that way. The
routes keep strahd's names (`/diario`, `/personaggi`, `/png`); only the visible
labels changed (Capitoli, Investigatori, Comprimari).

The site title is a generic "Call of Cthulhu" for now (with a "Stagione I"
kicker); it lives in `src/lib/site.ts`.

## Content conventions

These apply to every content edit.

- **Sources:** the season 1 transcript and photos of the handwritten notes
  live in `sources/` (gitignored). The handwritten notes win over the
  transcript for name spelling and for what the investigators actually
  learned. There will be no new sessions: content is a one-time import.
- **Renames and merges:** to rename an entity or merge duplicates, use the
  `rename-entity` skill (`.claude/skills/rename-entity/SKILL.md`). Never
  change a `titolo` or a filename by hand once published: wikilinks,
  backlinks and the page URL depend on them.
- **No real names:** the site is public and indexed. Players appear only
  as their investigators — never write a player's real name anywhere in
  `src/content/`, even though transcripts are full of them. Leave
  `giocatore` empty unless the user explicitly gives a name or nickname to
  show there.
- **Wikilinks:** ALWAYS link entities with Obsidian-style wikilinks:
  `[[Nome Entità]]` for plain text, `[[Nome Entità|testo visualizzato]]` for
  an alias. Use the entity's exact `titolo` (or its filename) as the target.
- **One canonical file per entity:** search the existing files before
  creating a new one; names with slight spelling variants must be unified
  into ONE file.
- **No spoilers:** only write what the investigators learned in play. Leave
  out Keeper asides, out-of-character explanations, and anything only one
  investigator learned in secret unless it was shared with the group.
- **Marginalia:** a markdown blockquote (`> …`) renders as a handwritten
  note in the margin of the tome. Use it sparingly, e.g. for a memorable line
  from the handwritten notes.

### Frontmatter

- `sessions/sessione-NN.md`: `titolo`, `numero`, `data` (real session date),
  `estratto` (≤300 chars), `luoghiVisitati`, `tag`. Body: third-person
  Italian narrative in `##` sections by scene.
- `personaggi/` (investigators): `titolo`, `professione`, `stato`
  (`vivo`|`morto`|`folle`|`scomparso`|`ritirato`, or the feminine
  `viva`|`morta`|`scomparsa`|`ritirata` for women), `sanita` (final SAN,
  optional), `fazione`, `estratto`, `tag`, `immagine` only if the file
  exists.
- `png/` (comprimari): `titolo`, `ruolo`, `stato`
  (`vivo`|`morto`|`folle`|`scomparso`|`sconosciuto`, feminine
  forms as above), `fazione`, `luogo`,
  `estratto`, `tag`.
- `luoghi/`: `titolo`, `tipo`, `regione`, `estratto`, `tag`.
- Filenames are the kebab-case, accent-free `titolo`.

## Commits

Conventional commits, subject line only. Content updates use the `content`
scope, e.g. `feat(content): add sessione-03`.

## Don'ts

- Don't invent a second linking system: use only `[[...]]`, never manual
  markdown links for internal references between entities.
- Don't add a `slug` field to frontmatter.
- Don't install extra UI frameworks (no Tailwind/React/Vue) — the site uses
  plain CSS and pure Astro.

## Technical notes

- Wikilinks are resolved at build time by `src/lib/wikilinks.mjs` (slug map)
  and `src/lib/remark-wikilinks.mjs` (remark plugin). A wikilink to a missing
  entity doesn't break the build: it renders as a "broken link" and logs a
  `[wikilinks] Unresolved link` warning.
- Backlinks ("Menzionato in") are computed by `src/lib/backlinks.ts` from
  every entry's wikilinks, plus each session's `luoghiVisitati`.
- Session pages get margin notes on wide screens (not to be confused with the
  blockquote marginalia): `remark-wikilinks.mjs` lists, after each `##`
  heading, the comprimari and places mentioned there for the first time, with
  their `ruolo` (or `tipo`) and a † when `stato` is `morto`/`morta`. Keep
  `ruolo` short: it's what the margin shows.
- Astro caches rendered markdown in `node_modules/.astro/data-store.json` and
  doesn't notice changes to the remark plugins: delete that file after
  editing them, or old renders stick around.
- In `astro dev`, restart the dev server after adding new entities.
- Entry pages use their `estratto` as the meta description, so keep it a
  self-contained sentence.
- Investigator portraits live in `public/images/personaggi/`: `<id>.png`
  (square, referenced by `immagine`) and `<id>-full.png` (tall, shown on the
  home page instead of the "Dramatis personae" list when present).
- There's no map yet. A 1920s world map (US and Berlin) may come later;
  `luoghiVisitati` is kept on sessions for it.
- Search (`/cerca`) uses Pagefind: the index is generated by `pnpm build` and
  doesn't exist in `astro dev` — use `pnpm build && pnpm preview`.
