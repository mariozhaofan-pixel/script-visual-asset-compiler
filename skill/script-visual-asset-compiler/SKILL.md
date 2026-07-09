---
name: script-visual-asset-compiler
description: Extract production-ready visual asset plans, prompt documents, cinematic image prompts, and optional canvas CLI blueprints from scripts, outlines, novels, director breakdowns, or storyboards. Use when Codex needs to turn 剧本, 导演拆解, 分镜, 角色, 服装, 场景, 怪兽/生物, 载具, 色卡/视觉DNA, 关键道具, 生图提示词, 工业化提示词文档, scene continuity, same-location multi-angle images, doorway/interior/exterior scene plates, cinematic LOOK cards, LOOK transfer, canvas node planning, LibTV/小云雀/updream/neowow or generic CLI canvas workflows, concept art, reference-image planning, or AI image asset requests into structured characters, costumes, scenes, props, creatures, vehicles, color/material DNA, storyboards, video segments, scene-view prompts, shot-frame prompts, clean image-generation prompts, and optional canvas node payloads; also use when the user asks to generate the corresponding character, scene, prop, creature, vehicle, storyboard, video, or cinematic frame images.
---

# 剧本资产提取器

## Operating Goal

Turn a script into a reusable visual asset package. The user-facing skill name is `剧本资产提取器`; keep the technical skill ID as `$script-visual-asset-compiler` for Codex invocation compatibility.

```text
Script -> Director breakdown -> Continuity memory graph -> Key subjects -> Prompt documents -> Optional canvas blueprint / CLI node payload -> Optional image generation
```

The default final output is Chinese, production-oriented, and ready for image generation. Do not only summarize the plot. Extract a stable visual system that can generate consistent character, costume, scene, prop, creature, vehicle, color/material DNA, storyboard, video-segment, and optional cinematic frame images across later storyboard or video work.

If `$cinematic-video-prompt-compiler` is available and the user also wants full cinematic shot prompts, use it as the companion skill after this asset extraction. Do not depend on it for the core asset inventory.

## Reference Files

Read only the files needed for the current task:

| File | Read when |
|------|-----------|
| `references/cinematic/integration-rules.md` | When applying cinematic quality to extracted assets, deciding asset-safe vs full cinematic treatment, or resolving conflicts between clean reference assets and dramatic frames. |
| `references/cinematic/look-card-and-transfer.md` | When the user asks for LOOK CARD, LOOK transfer, style variants, named looks, or cinematic still frames. |
| `references/cinematic/presets-reference.md` | When a known style, film, show, DP-style, preset name, or "像...风格" request is named. |
| `references/cinematic/params-reference.md` | When assembling any cinematic prompt layer. |
| `references/cinematic/recipes-reference.md` | When the style request is fuzzy and no exact preset is available. |
| `references/cinematic/anti-slop-system.md` | When adding realism, anti-slop, or model-aware negative/positive constraints. |
| `references/cinematic/model-adapters.md` | When the user names GPT Image, Midjourney, Flux, SDXL, or another still-image model. |
| `references/production/asset-taxonomy-and-profiles.md` | When extracting creatures, vehicles, color/material DNA, storyboards, video segments, priorities, project profiles, or broad asset maps. |
| `references/production/prompt-document-compiler.md` | When the user wants industrial prompt documents, node-ready prompt docs, QA notes, rerun fixes, or versioned prompt packages. |
| `references/production/output-templates.md` | When the user wants the full default output structure, prompt-only output, generation manifest, or a file-ready package. |
| `references/canvas/generic-canvas-blueprint.md` | When the user asks for canvas planning, CLI node creation, LibTV, 小云雀, updream, neowow, or any generic infinite-canvas CLI workflow. |

## Input Handling

Accept scripts, outlines, dialogue, novels, shot lists, storyboards, reference images, existing prompts, or mixed materials.

When reference images are provided, assign explicit roles before extraction:

```text
@[image-1] as character identity / costume / scene / prop / palette / scene-angle reference
```

State what each reference controls and what it does not control. If a reference conflicts with the script on identity, costume, period, setting, geography, prop function, or doorway/interior/exterior relation, ask whether to preserve the reference or preserve the script.

Do not ask clarifying questions unless missing information would materially change the asset set or break spatial continuity. Make conservative assumptions and label them briefly.

## Source-of-Truth Model

Keep four layers separate:

1. `Base Truth`: script facts, confirmed references, user constraints, and stable asset IDs.
2. `Continuity Context`: adjacent scenes, recurring locations, portals, screen direction, style lock, previous generated assets, and reference-image roles.
3. `Derived Prompts`: per-asset and per-view image prompts derived from Base Truth plus Continuity Context.
4. `Cinematic LOOK Layer`: optional camera, lens, light, color, texture, mood, realism, and anti-slop treatment. It can enrich prompts but never override Base Truth.
5. `Production Prompt Document`: optional versioned document containing prompt body, parameters, QA notes, and rerun fixes.
6. `Canvas Blueprint / CLI Payload`: optional vendor-neutral plan for creating canvas nodes through an already available CLI.
7. `Submission Payload`: the final concise prompt and selected reference images sent to an image generator.

Do not let generated images silently rewrite Base Truth. If a generated asset contradicts the script, mark it as a candidate that needs acceptance, revision, or rejection.

## Workflow

1. Read the script for story function, not just nouns. Identify who changes, where action happens, and which objects move the plot.
2. Build a director breakdown:
   - premise and genre
   - time period and world rules
   - scene geography and recurring locations
   - character power relationships and visual contrast
   - recurring motifs, prop logic, and reveal moments
3. Build a compact storyboard map:
   - scene order and story beats
   - which characters, costumes, locations, props, and scene states appear in each beat
   - what must stay visually consistent across beats
4. Build a continuity memory graph:
   - characters, costumes, scenes, props, and their stable IDs
   - scene zones, entrances, exits, doors, windows, hallways, stairs, vehicles, counters, beds, tables, altars, screens, or other fixed landmarks
   - screen-left/screen-right axis, movement direction, doorway direction, and camera-parent relationships when a scene needs multiple views
5. Extract key subjects into stable IDs:
   - `CHR-01` characters
   - `CST-01` costumes or costume states when one character has multiple looks
   - `CRE-01` creatures, monsters, animals, mechanical beasts, mounts, mutated life, or non-human entities
   - `SCN-01` canonical scenes / locations / location states
   - `SCN-01_VIEW-01_PRIMARY` / `SCN-01_VIEW-02_REVERSE` derived scene views from the same canonical scene
   - `PRP-01` plot props, weapons, tokens, letters, devices, vehicles, artifacts, repeated objects, or visually iconic objects
   - `VEH-01` vehicles, ships, cars, aircraft, mecha, bikes, carts, or other transport systems
   - `DNA-01` color palette, material DNA, lighting DNA, and visual bible assets
   - `STB-01` storyboard grids or sequence boards
   - `VID-01` video segment plans only when video prompting is requested
   - `SHOT-01_FRAME-01` cinematic keyframes only when the user requests frames with characters, posters, or storyboard stills
6. Decide the output mode:
   - prompt-only for Codex/GPT chat
   - production prompt documents
   - canvas blueprint only
   - CLI command plan
   - execute an already available canvas CLI only when explicitly requested
7. Decide the cinematic treatment:
   - clean asset reference only
   - scene plate with cinematic LOOK
   - full cinematic frame prompt
   - LOOK CARD first because the style request is fuzzy
   - LOOK transfer from a named preset/style
8. Write image-generation prompts or prompt documents for each subject. Each prompt must be usable without reading the whole script.
9. If the user asks to generate images or create canvas nodes, execute in dependency order: project profile / DNA, character identity/costume, creatures/vehicles/props, scene master plates, derived scene views, storyboard/keyframes, video segments.

## Extraction Rules

Characters:

- Extract named characters and unnamed characters with visual or plot importance.
- Include visible age range, body type, facial features, hair, expression baseline, posture, social class, and visual temperament only when supported by the script or required for generation.
- Include every costume state that matters to continuity: everyday outfit, uniform, disguise, damaged state, ceremonial outfit, wet/bloody/dusty state, etc.
- Keep identity separate from clothing so a character can be reused with multiple costumes.

Scenes:

- Extract every distinct location that appears on screen, including repeated locations with materially different states, such as day/night, destroyed/intact, crowded/empty, clean/rainy.
- Treat a scene as a spatial asset, not a mood label. Describe layout, scale, zones, entrances/exits, portal relationships, fixed landmarks, material surfaces, lighting source, atmosphere, color palette, and objects fixed in the set.
- Scene asset prompts must be location plates with no people.
- Do not describe characters, acting, or dialogue in scene asset prompts.

Props:

- Extract props that drive plot, reveal identity, repeat across scenes, create conflict, mark status, or need continuity.
- Include invitation letters, weapons, heirlooms, tokens, phones, contracts, medicine, jewelry, masks, devices, vehicles, ritual objects, special containers, and clue objects.
- Describe scale, material, color, wear, marks, inscriptions, damage, mechanism, and the prop's story function.
- Do not include generic furniture or background clutter unless it becomes important in action, blocking, recognition, or spatial continuity.

Broader production assets:

- Extract `CRE-*` when the script contains monsters, animals, mechanical beasts, mounts, unknown life forms, mutated organisms, or creature-like threats.
- Extract `VEH-*` when a vehicle or transport system has visual continuity, action importance, world-building value, or generation difficulty.
- Extract `DNA-*` when a project needs a shared color palette, material system, lighting rule, visual bible, or profile lock before generating multiple assets.
- Extract `STB-*` when the user needs a storyboard grid, beat board, contact sheet, or sequence-level visual plan.
- Extract `VID-*` only when the user asks for video prompt planning. Do not start final video generation unless explicitly requested.

## Cinematic Treatment Rules

Cinematic style is a derived prompt layer. It must improve image quality without corrupting asset extraction.

Use `references/cinematic/integration-rules.md` when deciding how much cinematic treatment to apply. In short:

- Character and costume reference assets stay on a clean solid-color background. Add realistic skin, hair, textile, material, and optical clarity, but do not add film sets, smoke, random foreground, dramatic action, or lens flare.
- Prop reference assets stay on a clean solid-color background. Add material fidelity, wear, construction details, and scale logic, but do not add hands or scene environments.
- Scene assets can use full cinematic LOOK treatment because they are environment plates. They must still be empty, geographically readable, and consistent across derived views.
- Shot frames can use the full cinematic still-image layer only when the user requests character-containing frames, posters, keyframes, or storyboard images. Label them as `SHOT-*`, not `SCN-*`.

When the user names a style, preset, film, show, DP-style, or says "像...风格", read `references/cinematic/presets-reference.md` and transfer the LOOK layer only. Preserve script facts, asset IDs, clean-background requirements, no-people scene plates, and spatial continuity.

When the user gives a fuzzy style request, read `references/cinematic/recipes-reference.md` and `references/cinematic/look-card-and-transfer.md`. Output a LOOK CARD first unless the user explicitly asks to proceed automatically.

When assembling cinematic prompts, read `references/cinematic/params-reference.md`, `references/cinematic/anti-slop-system.md`, and `references/cinematic/model-adapters.md` as needed. Use anti-slop as a compact, subject-aware clause, not a long generic negative prompt.

## Prompt Document and Canvas Modes

Default to `prompt_only` unless the user asks for a canvas, nodes, CLI execution, or a project board.

Use `references/production/prompt-document-compiler.md` when the user wants structured prompt documents. Keep these layers separate:

- `Prompt Body`: only model-facing image/video prompt text.
- `Parameters`: aspect ratio, model, quality, reference images, canvas section, node metadata.
- `Production Notes`: QA checklist, rerun fixes, version suggestions, operator instructions.

Do not put production notes, CLI commands, node metadata, or rerun instructions inside the model prompt body.

Use `references/canvas/generic-canvas-blueprint.md` for canvas workflows. Support these modes:

- `prompt_only`: output asset prompts only.
- `blueprint_only`: output canvas plan and node content, do not run CLI.
- `cli_command_plan`: output safe commands or JSON payload plan, do not execute.
- `execute_cli`: run a detected/provided canvas CLI only when the user explicitly asks to create/update canvas nodes.

Canvas planning must be vendor-neutral. LibTV, 小云雀, updream, neowow, and custom canvas tools are all treated as CLI canvas targets when a usable executable and command syntax are available. Do not hard-code one vendor.

CLI safety:

- Never install a canvas CLI as part of this skill.
- Never read, print, modify, or write API keys or access tokens.
- Only check executable existence and help/version output when needed.
- If CLI syntax is unknown, stop at `cli_command_plan` or ask for the tool's node-creation syntax.
- If no CLI is available, still return the asset list, prompt documents, and optional canvas blueprint.

## Scene Continuity and Multi-View Rules

Create a scene master plate for every important location. Add derived views only when the script needs them.

Create derived scene views when any of these are true:

- the same location appears in multiple beats or episodes
- action crosses a door, gate, window, car door, elevator, stairwell, corridor, or threshold
- the story needs inside/outside, room/corridor, upstairs/downstairs, front/back, reverse shot, or over-the-shoulder geography
- a reveal depends on what is visible from one angle but hidden from another
- a chase, confrontation, search, entrance, exit, or eavesdropping beat depends on path continuity
- later image/video generation explicitly needs consistent scene references beyond the master plate

Use this view hierarchy:

```text
SCN-01_MASTER                  canonical wide view / spatial truth
SCN-01_VIEW-01_PRIMARY          primary story-facing angle
SCN-01_VIEW-02_REVERSE          reverse angle, if needed
SCN-01_VIEW-03_INSIDE_TO_OUT    inside looking outside through the threshold, if needed
SCN-01_VIEW-04_OUTSIDE_TO_IN    outside looking inside through the threshold, if needed
SCN-01_VIEW-05_APPROACH_PATH    path/entry/corridor/stair/vehicle approach view, if needed
SCN-01_DETAIL-01                key fixed landmark or prop zone, if needed
```

Do not generate all view types by default. For each scene, output only the views needed by the script and give a short reason for every included derived view. If a view type is unnecessary, omit it instead of listing it with empty content.

For every multi-view scene, output a `场景连续性卡`:

- `空间母版`: one-sentence canonical scene identity.
- `平面关系`: describe left/right/front/back, depth, routes, and zones in words.
- `固定锚点`: doors, windows, furniture, signage, lamps, cracks, stairs, counters, vehicles, trees, skyline, or other objects that must not move between views.
- `门槛/连接点`: for door scenes, define what is on both sides, door swing direction, handle side, wall material, floor transition, and what can be seen through the opening.
- `光线逻辑`: source, direction, time, weather, spill from adjacent space, and material reaction.
- `风格锁`: lens feel, color palette, texture, realism level, art direction.
- `视角树`: root/master view and child views, with a short reason for each derived view.

Scene view prompts must explicitly preserve the master scene:

```text
继承 SCN-01_MASTER 的空间结构、材质、门窗位置、固定锚点、光线方向和风格；只改变相机位置与可见区域。
```

Doorway / threshold prompts must lock both sides:

```text
门内与门外属于同一连续空间，门框、门把手方向、地面材质交界线、墙面颜色、光线溢出方向保持一致；从门外看见室内的同一锚点，从门内看见门外的同一走廊/街道锚点。
```

For same-scene multi-angle prompts:

- Generate the canonical master wide plate first.
- Use the master plate as the preferred scene reference for derived angles when an image tool supports references.
- If no tool reference is available, repeat the scene continuity card in concise form inside each derived prompt.
- Keep characters out of all scene asset images unless the user explicitly asks for shot frames instead of scene plates.
- Do not let a derived angle introduce new doors, windows, furniture, props, lighting sources, signage, or weather that contradict the master plate.

Keep scene plates separate from shot frames:

- `SCN-*` outputs are empty environment/location assets.
- If the user asks for a frame with characters, create a `SHOT-xx_FRAME-*` or storyboard/keyframe prompt that references `SCN-*`, `CHR-*`, `CST-*`, and `PRP-*`; do not relabel it as a scene asset.
- If a location is normally crowded, first create the empty scene plate. Treat crowd/extras as a separate background performer group or shot-frame layer only when needed.

## Output Structure

Use a compact structure by default: director breakdown, storyboard asset map, continuity graph, project profile/DNA, cinematic LOOK layer, assets by type, optional prompt documents, optional Canvas Blueprint, gaps/assumptions, and QA summary.

When the user wants only prompts, omit long analysis but keep asset IDs, scene continuity cards, and prompts. When the user wants the full template, prompt-only format, or manifest, read `references/production/output-templates.md`. When the user wants a file artifact, save the result as Markdown and include a manifest mapping IDs to generated image filenames.

## Image Prompt Requirements

All prompts must be self-contained. Each prompt must name:

- subject ID and subject name
- visual identity or set identity
- costume / material / color / texture
- view angle and framing
- lighting and background requirement
- continuity anchors
- negative constraints

Character asset prompts must include:

- clean solid-color background
- full-body front view for `正视图`
- full-body back view for `背视图`
- head-and-shoulders portrait for `大头照`
- neutral standing pose unless the user requests acting poses
- same identity, same costume, same proportions across all views
- costume-state ID such as `CST-01` when a character has multiple required looks; create one front/back/head prompt set per required costume state
- no extra characters, no text, no watermark, no logo

Scene asset prompts must include:

- wide panoramic establishing view for the master plate
- no people, no crowds, no character silhouettes
- clear readable geography: foreground, midground, background, entrances, exits, portals, key set pieces, fixed landmarks
- lighting source, time of day, weather, material reaction, and color palette
- for derived views, explicit inherited anchors from the master plate and what changes in camera position
- the selected LOOK layer when the user asks for cinematic quality or a style has been inferred
- no text labels, no UI, no watermark

Prop asset prompts must include:

- clean solid-color background
- `45°侧视图` prompt showing front/side form and important details
- `背视图` prompt showing rear construction and continuity marks
- material, scale, wear, mechanism, unique marks, and story-use details
- no hands unless the user asks for scale reference; no text labels, no watermark

Creature asset prompts must include:

- species or construction logic
- side, front, top-down, and 3/4 view when a full design sheet is requested
- scale, anatomy/mechanical structure, surface material, signature visual memory point, functional story elements
- same creature across all views; no random species drift

Vehicle asset prompts must include:

- side, front, top, and 3/4 view when a full design sheet is requested
- silhouette, scale, movement system, cockpit/control interface, materials, decals/customization, damage/weathering
- same vehicle across all views; no toy-like or generic 3D render drift

Color/material DNA prompts or specs must include:

- color names, functions, and optional hex codes when useful
- material system
- lighting DNA
- inheritance notes for characters, scenes, props, creatures, vehicles, storyboards, and video segments
- forbidden drift

Shot-frame prompts must include:

- `SHOT-*` ID and the referenced `CHR-*`, `CST-*`, `SCN-*`, and `PRP-*` IDs
- one clear visual center when multiple characters or objects appear
- scene moment, framing, foreground depth if needed, camera/lens, lighting, color/texture, mood, realism, and anti-slop
- continuity inheritance from the scene master and character/prop assets
- no contradiction with clean reference assets; a dramatic frame does not replace CHR/SCN/PRP asset truth

Storyboard and video prompts must include:

- referenced asset IDs
- sequence or segment function
- continuity lock for character, costume, scene, props, and lighting
- panel list for storyboard grids or timecoded action timeline for video segments
- no final video generation unless explicitly requested

Canvas node payloads must include:

- node name, node type, asset reference, section, action, generate_now flag, and content source
- prompt body separated from parameters and QA notes
- node index table in final response when canvas output is created or planned

Use concise negative constraints at the end:

```text
限制: 纯色干净背景, 单一主体, 无文字, 无水印, 无logo, 无额外人物, 不改变角色身份/服装/比例
```

For scenes, replace the pure-background constraint with:

```text
限制: 无人物, 无人影, 无群众, 无文字, 无水印, 无logo, 不出现角色, 不出现与时代/剧情无关的物件, 不改变场景固定锚点/门窗位置/光线方向/材质关系
```

## Prompt Compression Priority

Final prompts should be complete but not bloated. Preserve guidance in this priority order:

1. asset ID, subject name, and required view name
2. character identity / scene master anchors / prop form anchors
3. view-specific camera position and what changed from the master view
4. doorway, route, screen direction, or continuity constraints that would visibly break if omitted
5. style, lighting, color, material, and atmosphere
6. concise negative constraints

If a final prompt becomes too long, remove decorative adjectives and low-impact mood language before removing spatial anchors, ID locks, or reference roles.

## Optional Image Generation

If the user asks to generate images:

1. Confirm the requested scope only when the count is unclear or excessive.
2. Generate separate images for each required view.
3. Use dependency order:
   - character identity and costume plates
   - `SCN-xx_MASTER` scene master plate
   - derived scene views using the master as reference when possible
   - prop views
4. Keep each generated image mapped to its ID and view name, for example `CHR-01_front`, `CHR-01_back`, `CHR-01_head`, `SCN-01_MASTER`, `SCN-01_VIEW-03_INSIDE_TO_OUT`, `SCN-01_VIEW-04_OUTSIDE_TO_IN`, `PRP-01_45deg`, `PRP-01_back`.
5. If an image-generation tool supports reference images, use character references for identity only, costume references for clothing only, scene references for environment/geometry only, and prop references for object form only.
6. After generation, report the output paths or visible images with the ID map.

## Quality Gate

Before finalizing, verify:

- Every named or visually important character has identity and costume coverage.
- Every scene appearing in the script is represented, including changed states of the same place.
- Important repeated or threshold-based scenes have a master scene plate and only the derived views the script actually needs.
- Doorway/interior/exterior prompts preserve both sides of the same connection.
- Scene plates and character-containing shot frames are not mixed under the same `SCN-*` asset.
- Every plot-driving or repeated prop is included.
- Characters, costumes, scenes, scene views, and props have stable IDs and cross-reference the storyboard map.
- Creatures, vehicles, color/material DNA, storyboard grids, and video segments are included when story-relevant or requested.
- Cinematic LOOK choices are treated as derived prompt layers, not script facts.
- Character prompts include front view, back view, and headshot.
- Scene prompts are no-people location plates; master and derived views do not contradict each other.
- Prop prompts include 45° side view and back view.
- Shot-frame prompts, when requested, are labeled as `SHOT-*` and reference existing CHR/CST/SCN/PRP IDs.
- Clean character and prop reference prompts are not polluted by cinematic background action, random foreground objects, or dramatic scene lighting.
- Fuzzy cinematic style requests produce a LOOK CARD first unless the user explicitly asks to proceed automatically.
- Named-look transfer preserves the user's script facts and only transfers camera, lens, light, color, texture, mood, and anti-slop behavior.
- Industrial prompt documents keep prompt body, parameters, QA notes, and rerun fixes separate.
- Canvas Blueprint is generated before any CLI node creation.
- CLI execution is skipped unless the user explicitly requests canvas node creation/update and a safe executable plus command syntax are available.
- No API keys, access tokens, or credential values are read, printed, modified, or written.
- Prompts are clean, literal, and image-generation ready, not vague mood labels.
- Reference-image roles and conflicts are explicit when references exist.
- Guidance is prioritized, not bloated: preserve at most the highest-impact continuity constraints inside each final prompt.
- No invented prop, costume, scene, or spatial relation is presented as script fact; assumptions are labeled.
