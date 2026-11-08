# Repo conventions

## Code style

- **No comments in code.** Names and structure carry the meaning. This applies to `.ts`, `.tsx`,
  `.mjs`, `.css`, and config files.
- Prettier is authoritative: single quotes, 100 columns, Tailwind class sorting.
- TypeScript strict. No `any`.
- Import app code through the `@/*` alias, never deep relative paths.

## Content

- Blog posts live at `content/blog/<YYYY>/<MM>/<DD>/<slug>.md`, mirroring the live URL.
- Pages live at `content/pages/<slug>.mdx`.
- `permalink` in frontmatter is the source of truth for routing. It is carried verbatim from the
  WordPress original and must never be regenerated from the file path.
- Structured data belongs in `data/*.yaml`, validated by `schemas/*.json`.
- How to add posts, pages, TSC members and navigation entries: `docs/README.md`.

## Build

- `output: 'export'`. Everything is static; there is no server at runtime.
- Content is read with `fs` at build time only.
- `public/img/` is generated from `assets/uploads/` by `tools/publish-assets.mjs` and is gitignored.

## Migration

The WordPress migration is finished and its tooling was removed. There are no redirects: an old
address without a page here returns 404, so link to the current permalink, never to an old one.
