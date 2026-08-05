# Bodega One Model Catalog

Versioned model-catalog JSON consumed by the Bodega One app's remote catalog refresh (Settings -> Models -> Automatic catalog updates) and by the website.

- `v1/model-catalog.json` - legacy shape: local (Ollama) + cloud model lists. Fallback when v2 is unavailable.
- `v2/catalog-bundle.json` - full bundle: model catalog + llama.cpp GGUF catalog + cloud picker overlay.

These files are GENERATED from the app repo's bundled configs by `apps/desktop/backend/scripts/generate-catalog-bundle.ts` - do not hand-edit here; changes land in the app repo first, then the regenerated output is committed here. The app validates every fetch against a schema firewall and always falls back to its bundled catalog on any failure. Contents are public model metadata only (names, sizes, Hugging Face repo ids, context windows). No telemetry is involved in fetching this file.

## Publishing rule

**Bump `version` AND `lastUpdated` in the app repo's `configs/model-catalog.json` whenever you change its contents, before regenerating.**

The app only accepts a fetched catalog when `ModelCatalogService.isNewer` sees a newer `lastUpdated`, then a newer `version`. Publishing changed content under an unchanged version does not fail anywhere — the fetch returns 200, the schema firewall passes, and the refresh reports `not-newer`. The update simply never reaches anyone, silently.

That happened between 2026-06-13 and 2026-08-05: the files here were missing two cloud models the app already shipped, while claiming the same version as the app's bundled copy.

Verify before and after publishing, from the app repo:

    cd apps/desktop/backend
    npm run catalog:build    # regenerate v1 + v2 into build/catalog/
    npm run catalog:check    # compares what is published here against that checkout

`catalog:check` exits non-zero when the published content differs from the app's while claiming the same version.
