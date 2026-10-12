# Page CSS lost after client-side navigation with `router.resolution: 'server'`

Minimal SvelteKit 3.0.1 app with `router.resolution: 'server'` and the default `paths.relative: true`. Only `/a` has CSS: its paragraph is red.

```sh
npm install
npm run build
npm run preview
```

1. Open http://localhost:4173/x/y/z (a full page load).
2. Click "go to /a".

Expected: the text on `/a` is red.
Actual: the text is black. The browser requested `/x/_app/immutable/assets/<hash>.css` and got a 404.

For comparison, open http://localhost:4173/ and click "/a": the text is red.

`/a/__route.js` calls `load_css(['../_app/immutable/assets/<hash>.css'])`. The `../` is relative to `/a/__route.js`, but `load_css` resolves it against the current page, `/x/y/z`.

Workaround: set `paths: { relative: false }`.
