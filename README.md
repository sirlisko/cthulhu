# Call of Cthulhu — season 1

Companion site for a finished, Italian-language *Call of Cthulhu* (7th
edition) campaign: chapters, investigators, NPCs and places, cross-linked with
Obsidian-style wikilinks. Built with [Astro](https://astro.build), content in
Markdown. Forked from [strahd](https://github.com/sirlisko/strahd).

The site itself is in Italian; code and docs are in English. Editorial
conventions (frontmatter, wikilinks, spoilers) live in
[`CLAUDE.md`](./CLAUDE.md).

## Development

Requires Node.js 22.12+ and [pnpm](https://pnpm.io).

```bash
pnpm install
pnpm dev       # http://localhost:4321
pnpm build     # static output in dist/ (plus the Pagefind search index)
pnpm preview   # serve the dist/ build
```

Deployed on Netlify (`netlify.toml`).

## Content layout

```
src/content/
  sessions/     chapters (sessione-NN.md)   → /diario/
  personaggi/   investigators               → /personaggi/
  png/          NPCs (comprimari)           → /png/
  luoghi/       places                      → /luoghi/
  riassunto/    campaign overview           → /riassunto/
```

Any entry can link to another with `[[Entity Name]]` (or
`[[Name|display text]]`). A blockquote renders as a handwritten margin note.

## License

- **Code** is licensed under [MIT](./LICENSE).
- **Campaign content** (everything under `src/content/` and `public/images/`)
  is licensed under [CC BY-NC 4.0](./LICENSE-CONTENT).

Call of Cthulhu is a registered trademark of Chaosium Inc. This is unofficial
fan material, not approved or endorsed by Chaosium.
