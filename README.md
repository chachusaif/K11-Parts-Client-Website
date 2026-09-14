# K11 JDM Parts

A one-page site for a sole trader who sources Japanese-market parts for the K11 Nissan Micra and March (1992–2002) and sells them through eBay.

<img width="1109" height="632" alt="image" src="https://github.com/user-attachments/assets/b24849a8-f9e5-4b46-aedd-95376893012c" />


## The brief

The client had no web presence beyond a Facebook profile and an eBay seller page. Buyers had no way to see the range he could source, and he was answering the same fitment questions by message all day. He needed something to link from his listings that made the operation look legitimate and cut down repeat questions.

## What I built

- A static page in plain HTML, CSS and JavaScript — no framework, no build step, no dependencies. It costs nothing to host and anyone can pick it up and edit it.
- A searchable parts index driven by one JavaScript array. Adding a part means copying a line, so the client maintains the catalogue himself without touching markup.
- Live text search across name, description and fitment, plus category filters, with an `aria-live` result count and a written empty state that routes people to Facebook rather than dead-ending them.
- Fitment data (chassis codes, engine codes, stud pattern) surfaced in the hero, because it's the first thing a buyer checks.

## Structure

```
index.html    markup and content
styles.css    design system in custom properties, mobile-first
script.js     parts data, search and filter, rendering
```

## Decisions worth explaining

**No framework.** The dynamic behaviour is one `filter()` call and a render loop over about thirty rows. React would have added a build step, a deploy pipeline and a dependency tree the client can't maintain, in exchange for nothing this doesn't already do. It would start to earn its place with shared state across views, routing, or a list long enough to need keyed reconciliation.

**Data separated from markup.** The catalogue lives in a single array of objects rather than hand-written table rows, so there's one source of truth and the shape is a straight lift to `parts.json` or a database row later. The category lookup falls back gracefully, because a non-technical maintainer editing a text file will eventually mistype a category and that shouldn't blank the page.

**Design pulled from Nissan's printed parts microfiche** — press-stock paper, ink, catalogue red — rather than the dark-mode-and-neon look most car sites default to. The audience restores twenty-year-old cars from OEM diagrams, so the reference is one they recognise.

**Accessibility as a floor, not a feature.** Semantic landmarks and a sequential heading outline, a skip link, `:focus-visible` rings, `aria-pressed` on the filter toggles so state is announced, an `aria-live` region so filtering gives screen reader feedback, `prefers-reduced-motion` respected, and contrast checked against WCAG AA.

## Running it

Clone it and open `index.html`. There's no build step.

Once the parts data moves to `parts.json`, `file://` will block the `fetch` on CORS, so serve it locally instead:

```
python3 -m http.server
```

## Deploying

Push to `main`, then Settings → Pages → deploy from branch `main`, folder `/ (root)`.

## Known limitations

The catalogue is hand-maintained, so it describes what the client can source rather than what's in stock or what it costs. A buyer still has to cross-reference eBay for live availability. That's the main thing the planned back end is meant to fix.

There are no automated tests. The matching logic reads filter state from module scope, which needs extracting into a parameter before it's usefully testable.

## Roadmap

- [ ] Photography of actual stock
- [ ] Move the parts array to `parts.json`
- [ ] Express + PostgreSQL back end with an admin page, so the client edits stock through a form rather than a text file
- [ ] eBay API integration on a schedule, with credentials held server-side, so the public catalogue reflects live listings
- [ ] Structured data (`Product` / `LocalBusiness` schema) for search results
