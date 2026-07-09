# Cinematic Integration Rules

Use these rules when the user wants cinematic quality, a named look, high-end still-image prompts, model-specific image prompts, or when the extracted asset prompts feel too plain for production use.

## Asset-Type Routing

Keep the script asset inventory as the source of truth. Cinematic style is a derived layer, never a replacement for asset IDs, identity locks, scene geography, or prop continuity.

### Characters: CHR / CST

Use cinematic enrichment only as material realism:

- Keep clean solid-color background.
- Keep front view, back view, and headshot requirements.
- Keep neutral pose unless the user asks for acting poses.
- Add skin, hair, textile, leather, metal, dirt, wetness, damage, and optical clarity anchors.
- Do not add cinematic sets, foreground objects, dramatic shadows, lens flares, smoke, rain, or story action unless the user asks for a character poster or frame.
- Use only anti-slop Tier A plus model adaptation. Skip Tier B genre clauses unless the character asset is explicitly a cinematic still.

### Scenes: SCN / SCN_VIEW

Use full cinematic enrichment for empty location plates:

- Build `SCN-xx_MASTER` first as spatial truth.
- Select or infer a LOOK layer from the story genre, time, weather, light source, and user style request.
- Lock the same LOOK layer across derived views unless the script state changes.
- Keep all scene plates empty: no characters, no crowds, no silhouettes.
- Use full anti-slop Tier A + one Tier B bucket + model adaptation.
- If a derived view changes only camera position, inherit the master scene structure, material, fixed anchors, door/window position, weather, and light direction.

### Props: PRP

Use cinematic enrichment as object realism:

- Keep clean solid-color background.
- Keep 45-degree side view and back view.
- Add material fidelity, wear, fingerprints, engraving, scratches, mechanism, patina, edge damage, and scale cues only when useful.
- Do not add hands, table settings, dramatic scene lighting, or random environment unless requested.
- Use only anti-slop Tier A plus model adaptation.

### Shot Frames: SHOT

Create SHOT outputs only when the user asks for frames with characters, cinematic stills, keyframes, posters, or storyboard frames.

- Reference existing CHR, CST, SCN, SCN_VIEW, and PRP IDs.
- Use complete cinematic assembly: scene, framing, foreground, camera, lighting, color/texture, mood, realism, anti-slop, and model note.
- Do not relabel a shot frame as a scene asset.

## LOOK Layer

A LOOK layer contains:

- camera body, lens, aperture
- light source, lighting style, fill ratio
- palette, saturation, film/color behavior, grain/texture, halation/glow
- mood
- anti-slop bucket

When using a named preset, load `presets-reference.md` and copy the preset's LOOK values. When no named preset exists, build a custom LOOK from `recipes-reference.md` and `params-reference.md`.

## Assembly Priority

When prompts get too long, preserve this order:

1. asset ID and required view
2. script truth and continuity anchors
3. character identity / scene spatial anchors / prop form anchors
4. view-specific camera position
5. LOOK layer: lighting, palette, material, texture, mood
6. realism and anti-slop
7. model notes

Drop decorative adjectives before dropping spatial anchors, identity locks, or prop construction details.

## Conflict Rules

- Clean asset references beat cinematic drama for CHR, CST, and PRP assets.
- Scene geography beats lens mood for SCN assets.
- User-specified style beats inferred style, unless it contradicts script truth.
- If a preset's default framing conflicts with a scene master or required asset view, keep the asset view and transfer only the LOOK layer.
- Do not use film grain, haze, halation, bloom, and shallow depth of field all at full strength. Keep at most two strong softening signals.

## Reference Loading

- Read `presets-reference.md` when the user names a look, show, film, DP-style, genre preset, or uses "像...风格".
- Read `params-reference.md` whenever assembling a cinematic prompt.
- Read `recipes-reference.md` when the style request is fuzzy and there is no exact preset.
- Read `anti-slop-system.md` whenever adding realism or anti-slop clauses.
- Read `model-adapters.md` when the user names GPT Image, Midjourney, Flux, SDXL, or another still-image model.
