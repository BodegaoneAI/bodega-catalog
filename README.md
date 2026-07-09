# Bodega One Model Catalog

Versioned model-catalog JSON consumed by the Bodega One app's remote catalog refresh (Settings -> Models -> Automatic catalog updates) and by the website.

- `v1/model-catalog.json` - legacy shape: local (Ollama) + cloud model lists. Fallback when v2 is unavailable.
- `v2/catalog-bundle.json` - full bundle: model catalog + llama.cpp GGUF catalog + cloud picker overlay.

These files are GENERATED from the app repo's bundled configs by `apps/desktop/backend/scripts/generate-catalog-bundle.ts` - do not hand-edit here; changes land in the app repo first, then the regenerated output is committed here. The app validates every fetch against a schema firewall and always falls back to its bundled catalog on any failure. Contents are public model metadata only (names, sizes, Hugging Face repo ids, context windows). No telemetry is involved in fetching this file.
