# Scene and Shot Prompt Grammar

Use this file when compiling stylized scene plates, derived scene views, storyboard frames, cinematic keyframes, or when a user provides a prompt-format reference and asks to unify scene prompt style.

This grammar is a prompt-body layer only. It does not replace asset extraction, stable IDs, scene continuity cards, canvas metadata, QA notes, or model parameters.

## Core Order

Compile `SCN-*`, `SCN_VIEW-*`, and `SHOT-*` prompt bodies in this order:

1. `SHOT TYPE / VIEW`: shot scale, camera angle, view role, and aspect/framing when relevant.
2. `SUBJECT & ACTION`:
   - for `SCN-*`: the subject is the empty location plate; action becomes spatial state only, not character action.
   - for `SHOT-*`: one clear visual center plus character action or story beat.
3. `ENVIRONMENT`: geography, time, weather, set zones, thresholds, foreground/midground/background, fixed anchors.
4. `COMPOSITION RULES - fixed`: composition choices that must not drift: symmetry, one-point perspective, negative space, foreground obstruction, depth staging, frame-within-frame, leading lines, camera-parent relation.
5. `IMMUTABLE STYLE KEYWORDS - highest weight`: the locked LOOK layer, style_strength, medium anchors, palette behavior, light behavior, material system, grain/texture, halation or haze if used.
6. `NEGATIVE / LIMITS`: concise exclusions targeted to the asset type and model.

Do not put execution notes, canvas node data, rerun advice, or QA text inside this prompt body.

## Unified Scene Plate Format

Use this skeleton for `SCN-*` and `SCN_VIEW-*`:

```text
<SCN-ID> <view name>, <shot type / camera view>, empty live-action location plate of <scene identity>, <environment and spatial state>.
Composition fixed: <foreground/midground/background>, <entrances/exits/thresholds>, <fixed anchors>, <composition rule>, <camera position>.
Continuity: inherit <SCN-ID_MASTER> spatial structure, materials, door/window positions, fixed anchors, light direction, weather, palette, and style; only camera position and visible area change.
Immutable style keywords: style_strength=<base/elevated/signature>, live-action location plate, practical full-scale set, natural lens perspective, <LOOK layer>, <palette behavior>, <motivated light>, <material system>, <film/optical texture>.
限制: 无人物, 无人影, 无群众, 无文字, 无水印, 无logo, 不出现角色, 不出现与时代/剧情无关的物件, 不改变场景固定锚点/门窗位置/光线方向/材质关系
```

For GPT Image or other natural-language models, keep this as fluent Chinese prose. For Midjourney-style engines, the last line may become a compact `--no` clause, but only when the engine supports it.

## Unified Shot / Storyboard Frame Format

Use this skeleton for `SHOT-*` and individual storyboard frames:

```text
<SHOT-ID>, 引用: <CHR/CST/SCN/PRP IDs>. <shot type> of <subject and action> in <environment>.
Composition fixed: <camera angle>, <framing>, <foreground depth or obstruction>, <visual center>, <screen direction>, <relationship to SCN master>.
Continuity: preserve <character identity/costume>, <prop form>, <scene anchors>, <light direction>, <time/weather>.
Immutable style keywords: style_strength=<base/elevated/signature>, cinematic live-action film still, <camera/lens medium anchor>, <motivated light>, <palette behavior>, <material/skin/fabric realism>, <film/optical texture>, <mood>.
限制: 无文字, 无水印, 无logo, 不改变角色身份/服装/比例, 不改变场景锚点和道具形态
```

Shot frames may contain characters and action. They must not be relabeled as `SCN-*` scene assets.

## Style Recipes To Extract From References

When a user wants a stronger scene or storyboard style but gives only fuzzy direction, select one recipe and adapt it to the script. Never copy example subjects, settings, or literal sample scenes into the user's project.

### Rainy Neo-Noir Practical Realism

Use for rain night, public scandal, pursuit, police, alley, street, window, or threshold scenes.

- foreground obstruction, wet glass, doorway, or silhouette layer
- anamorphic compression or controlled shallow depth when it does not damage scene readability
- high-contrast chiaroscuro, deep shadow pockets, practical street lamps or tungsten spill
- cold teal/cyan night palette balanced by warm practical highlights
- rain atmosphere, volumetric beams, wet pavement reflections, restrained halation
- avoid clean digital video, flat lighting, bright daylight, oversaturation, plastic CGI, artificial sharpness

### Large-Format Epic Isolation

Use for cosmic scale, divine court, ruins, desert, space, lonely character-in-vast-place, or mythic public-space scenes.

- ultra wide or wide establishing view, large negative space, strong horizon or axis geometry
- IMAX / large-format film feeling, deep focus or readable spatial depth
- desaturated earth, steel blue, ash grey, muted gold, pale highlight behavior
- naturalistic hard side light, overcast diffusion, practical motivated light
- photochemical texture, organic sharpness, slight halation, grounded realism
- avoid neon, glossy CGI, futuristic luxury unless script-specific, hyper-saturation, anime/cartoon, excessive bloom

### Quiet Youth / Memory Melancholy

Use for intimate school, rural edge, urban edge, memory, emotional aftermath, or fragile character beats.

- minimal composition, negative space, low or eye-level observational camera
- soft haze, restrained handheld feeling, gentle lens flare only when motivated
- cyan-blue night or warm backlit day, muted desaturated palette
- long soft shadows, quiet emotional distance, practical environment details
- avoid beauty-filter gloss, overdramatic fantasy glow, stock romance poster look

### Grounded Ancient Fantasy Portrait / Close Frame

Use mainly for character close-ups or shot frames, not empty scene plates.

- real skin pores, natural hair strands, tiny blemishes, moisture, fabric weave
- controlled studio or motivated set light, rim light only when physically plausible
- ethereal fantasy mood carried by costume materials, hair/makeup, jade/metal/silk textures, not by vague glow
- avoid wax skin, over-beautified faces, plastic CGI, unreadable costume clutter

## Scene-Specific Rules

- A scene prompt must stay spatially useful before it is beautiful.
- Master plates use wide/establishing or panoramic views by default.
- Derived views repeat only the continuity anchors needed to preserve the master.
- Strong foreground obstruction is allowed for `SHOT-*`; for `SCN-*`, use it only if the set remains readable.
- Do not stack every optical effect. Use at most two strong softening signals from grain, haze, halation, bloom, soft focus, rain, shallow depth of field.
- If a style recipe's default subject conflicts with the script, transfer only camera, lens, light, color, texture, and composition behavior.

## Conflict Handling

This grammar does not conflict with the existing scene asset format because it only standardizes the final prompt-body order.

Ask the user before merging only if a provided prompt format requires any of these:

- putting people into `SCN-*` scene assets
- removing stable asset IDs
- replacing scene continuity cards with pure mood prompts
- using readable labels, UI, zodiac/MBTI text, or brand marks as style carriers
- putting CLI plans, QA notes, or canvas metadata into the model prompt body
