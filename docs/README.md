# Editing openapis.org

Everything on the site is a file in this repository. Change it on GitHub or locally and open a pull
request. CI checks it; merging to `main` deploys automatically.

Preview locally: `npm ci`, then `npm run dev` and open <http://localhost:3000>.

## Add a blog post

1. Create `content/blog/<YYYY>/<MM>/<DD>/<slug>.md`.
2. Start it with:

   ```yaml
   ---
   title: Announcing Arazzo 1.2
   date: 2026-11-20
   author: sensiblewood
   category: blog
   tags: [arazzo, openapi]
   permalink: /blog/2026/11/20/announcing-arazzo-1-2
   ---
   ```

   - `author` is a `slug` from `data/authors.yaml`. For a new author, add `slug` and `name` there.
   - `category` is `blog`, `announcement`, `news` or `presentation`. Other categories do not appear
     on `/blog`.
   - `permalink` uses the same date as `date`. Never change it once published: there are no
     redirects, so the old URL would return 404.

3. Write Markdown below it. Start headings at `##`; the title comes from `title`.
4. A YouTube or SlideShare link alone in a paragraph becomes an embedded player.

## Add an image or document

Put the file in `assets/uploads/<YYYY>/<MM>/` and link to it under `/img/uploads/<YYYY>/<MM>/`.
PNG, JPG and GIF are converted to WebP, so `photo.jpg` is linked as `photo.webp`. SVG and PDF keep
their name. Never commit `public/img/`; it is generated.

```markdown
![Arazzo workflow](/img/uploads/2026/11/arazzo-workflow.webp)
```

## Update a page

Pages are `content/pages/**/*.mdx`. The URL is the `permalink` in the front matter, not the file
path. `layout` decides how the page is built:

- `prose`: a title and Markdown, e.g. `content/pages/faq.mdx`.
- `composed`: sections built from components (`Hero`, `Section`, `CardGrid` and `Card`, `CTA`,
  `Action`, `Quote`, `BenefitGrid` and `Benefit`). Copy a pattern from `content/pages/index.mdx`.

Give components string attributes only: `columns="4"` works, `columns={4}` is silently ignored.

A new page needs `title`, `permalink` and `layout`. Add it to the navigation if it should be found.

## Add or remove a TSC member

Edit `data/tsc-members.yaml`. The list appears on `/about/technical-developer-community`.

```yaml
current:
  - slug: lorna-mitchell
    name: Lorna Mitchell
    term: 2024–Current
    photo: /img/uploads/2025/05/lornajane.webp
former:
  - slug: tony-tam
    name: Tony Tam
    term: 2016–2017
```

- **Add:** append an entry to `current`. Use a square photo, added as described under images.
- **Remove:** move the entry to `former`, drop `photo`, and set `term` to the years served, e.g.
  `2024–2026`.

## Edit the main navigation

Edit `data/nav.yaml`. A top-level item has either `href` (a link) or `children` (a dropdown):

```yaml
- label: Participate
  children:
    - label: Calendar
      href: /get-involved/about
- label: Blog
  href: /blog
```

Links starting with `https://` open in a new tab and get the external-link icon automatically. The
LinkedIn and GitHub icons in the header come from `data/social.yaml`.

## Other content

| What                        | Where                                                                    |
| --------------------------- | ------------------------------------------------------------------------ |
| Testimonials                | `data/testimonials.yaml`, shown on `/testimonials`                       |
| Member logos                | `/membershipmembers` embeds landscape.openapis.org; change members there |
| Community calendar meetings | Managed in LFX; see below                                                |
| Closed calls for proposals  | `content/cfp/`, kept for reference                                       |

**Community calendar** (`/get-involved/about`): the _Calendar sync_ workflow copies the meetings
from LFX into `data/calendar.json` every six hours and deploys. Do not edit that file. To update
now, run _Actions → Calendar sync → Run workflow_.

## Before opening a pull request

```sh
npm run lint && npm run typecheck && npm run format:check
npm run validate && npm run build && npm run check:render
npm run test:e2e
```

CI runs the same checks on every pull request. Code conventions are in [`AGENTS.md`](../AGENTS.md).
Anything that loads from another site must wait for cookie consent: iframes in content do this
automatically, and a new script must check `useConsent()` like `src/components/Analytics` does.
