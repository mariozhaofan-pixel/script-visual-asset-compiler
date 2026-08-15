# Cinematic Integration Rules

Use these rules when the user wants cinematic quality, a named look, high-end still-image prompts, model-specific image prompts, stronger stylization, live-action film stills, or when extracted asset prompts feel too plain for production use.

Keep the script asset inventory as the source of truth. Cinematic style is a derived layer, never a replacement for asset IDs, identity locks, scene geography, prop continuity, or user-requested views.

## Asset-Type Routing

### Characters: CHR / CST

Default route: clean reference plate.

- Keep clean solid-color background.
- Use `CST-*` costume design sheets by default: front costume view, back costume view, and important clothing/material/detail windows in one controlled image.
- Keep `CHR-*` as the identity and performance lock; generate actor front/back/headshot views only when explicitly requested.
- Keep neutral pose or neutral fit-model stance unless the user asks for acting poses.
- Add skin, hair, textile, leather, metal, dirt, wetness, damage, and optical clarity anchors.
- Do not add cinematic sets, foreground objects, dramatic shadows, lens flares, smoke, rain, or story action unless the user asks for a character poster or frame.
- Use only anti-slop Tier A plus model adaptation.

Styled route: cinematic costume design plate, with optional casting plate only on request. Use `style-amplification.md` when the user asks for 真人电影, 影视风格, 定妆照, 服装设计图, 角色正面全身图, 风格化, 玄幻, low-saturation fantasy, or says the character output is not stylized enough.

- Keep one character/costume asset only, the requested view or costume-sheet layout, identity lock, costume lock, and no readable text.
- Use live-action actor language, practical costume department, real makeup/hair/skin/fabric/metal/jade textures, and a low-key plain studio or minimal stage background.
- Allow cinematic lighting, palette, subtle ground shadow, and visible project DNA in costume cut, halo, jewelry, embroidery, aura, or material motifs.
- Do not add story action, fighting poses, crowd, unrelated props, or full scene blocking unless the user asks for `SHOT-*`.

### Scenes: SCN / SCN_VIEW

Use full cinematic enrichment for empty location plates:

- Build `SCN-xx_MASTER` first as spatial truth.
- Compile the final prompt body with `scene-shot-prompt-grammar.md`: view type, empty location state, environment, fixed composition, immutable LOOK/style keywords, concise scene limits.
- Select or infer a LOOK layer from the story genre, time, weather, light source, and user style request.
- Lock the same LOOK layer across derived views unless the script state changes.
- Keep all scene plates empty: no characters, no crowds, no silhouettes.
- Use full anti-slop Tier A + one Tier B bucket + model adaptation.
- If a derived view changes only camera position, inherit the master scene structure, material, fixed anchors, door/window position, weather, and light direction.
- When the user asks for 真人电影, 影视风格, 玄幻低饱和, or stronger stylization, use live-action location plate / practical full-scale set language and actively exclude concept art, game environments, digital matte painting, UI panels, and readable glyphs.

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
- Compile the final prompt body with `scene-shot-prompt-grammar.md`: shot type, subject/action, environment, fixed composition, immutable LOOK/style keywords, concise limits.
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

For style-heavy requests, also assign `style_strength` from `style-amplification.md`. `signature` style requires visible style carriers, not just a LOOK CARD or mood adjectives.

## Abstract Visual Systems

When a script contains or the user requests zodiac, MBTI, personality tags, reputation heat, divine ranking, social-media heat, or similar systems, convert the system into visual grammar:

- geometry, wheels, grids, or orbit structures
- costume seams, embroidery, aura, halo, jewelry, weapon/prop construction, or makeup motifs
- scene architecture, floor inlays, ceiling star maps, mirror pools, empty chambers, or abstract light behavior
- no readable labels, type names, UI cards, platform logos, or text charts

## Assembly Priority

When prompts get too long, preserve this order:

1. asset ID and required view
2. script truth and continuity anchors
3. character identity / scene spatial anchors / prop form anchors
4. view-specific camera position or character view
5. style_strength and visible style carriers
6. LOOK layer: lighting, palette, material, texture, mood
7. realism and anti-slop
8. model notes

Drop decorative adjectives before dropping spatial anchors, identity locks, style carriers, or prop construction details.

## Conflict Rules

- Clean asset references beat cinematic drama for default CHR, CST, and PRP reference plates.
- User-requested cinematic casting plates beat the default clean reference route while still preserving identity, costume, view, and single-subject constraints.
- Scene geography beats lens mood for SCN assets.
- User-specified style beats inferred style unless it contradicts script truth.
- If a preset's default framing conflicts with a scene master or required asset view, keep the asset view and transfer only the LOOK layer.
- Do not use film grain, haze, halation, bloom, and shallow depth of field all at full strength. Keep at most two strong softening signals.

## Reference Loading

- Read `presets-reference.md` when the user names a look, show, film, DP-style, genre preset, or uses "像...风格".
- Read `params-reference.md` whenever assembling a cinematic prompt.
- Read `recipes-reference.md` when the style request is fuzzy and no exact preset is available.
- Read `style-amplification.md` when the user asks for stronger style, live-action film, 真人电影, 影视风格, 玄幻低饱和, visual DNA, zodiac/MBTI visualization, or says the result is too concept-art-like or not stylized enough.
- Read `anti-slop-system.md` whenever adding realism or anti-slop clauses.
- Read `model-adapters.md` when the user names GPT Image, Midjourney, Flux, SDXL, or another still-image model.
