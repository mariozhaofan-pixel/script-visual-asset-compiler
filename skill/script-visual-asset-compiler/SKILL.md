---
name: script-visual-asset-compiler
description: Extract production-ready visual asset plans from scripts, outlines, novels, director breakdowns, or storyboards. Use when Codex needs to turn 剧本, 导演拆解, 分镜, 角色, 服装, 场景, 关键道具, 生图提示词, scene continuity, same-location multi-angle images, doorway/interior/exterior scene plates, concept art, reference-image planning, or AI image asset requests into structured characters, costumes, scenes, props, scene-view prompts, and clean image-generation prompts; also use when the user asks to generate the corresponding character, scene, or prop images.
---

# 剧本资产提取器

## Operating Goal

Turn a script into a reusable visual asset package. The user-facing skill name is `剧本资产提取器`; keep the technical skill ID as `$script-visual-asset-compiler` for Codex invocation compatibility.

```text
Script -> Director breakdown -> Continuity memory graph -> Key subjects -> Image asset prompts -> Optional image generation
```

The default final output is Chinese, production-oriented, and ready for image generation. Do not only summarize the plot. Extract a stable visual system that can generate consistent character, costume, scene, and prop images across later storyboard or video work.

If `$cinematic-video-prompt-compiler` is available and the user also wants full cinematic shot prompts, use it as the companion skill after this asset extraction. Do not depend on it for the core asset inventory.

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
4. `Submission Payload`: the final concise prompt and selected reference images sent to an image generator.

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
   - `SCN-01` canonical scenes / locations / location states
   - `SCN-01_VIEW-01_PRIMARY` / `SCN-01_VIEW-02_REVERSE` derived scene views from the same canonical scene
   - `PRP-01` plot props, weapons, tokens, letters, devices, vehicles, artifacts, repeated objects, or visually iconic objects
6. Write image-generation prompts for each subject. Each prompt must be usable without reading the whole script.
7. If the user asks to generate images, generate in dependency order: character identity/costume first, scene master plates before derived scene views, props before shots that depend on them.

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

Use this structure by default:

```text
## 导演拆解
- 故事核心:
- 类型 / 时代 / 世界规则:
- 视觉基调:
- 角色关系与视觉对比:
- 场景逻辑:
- 道具逻辑:

## 分镜资产地图
| Beat | 剧情功能 | 角色 | 服装 | 场景/视角 | 关键道具 | 连续性备注 |

## 连续性记忆图谱
- 角色:
- 服装:
- 场景:
- 道具:
- 空间/门槛/路径:
- 风格锁:

## 角色资产
### CHR-01 角色名
- 角色功能:
- 形象锁定:
- 服装状态:
- 连续性要点:
- 生图提示词:
  - 正视图:
  - 背视图:
  - 大头照:

## 场景资产
### SCN-01 场景名
- 剧情功能:
- 空间母版:
- 场景连续性卡:
- 必需视角:
- 生图提示词:
  - SCN-01_MASTER 全景无人物:
  - SCN-01_VIEW-01_PRIMARY 主剧情视角:
  - SCN-01_VIEW-02_REVERSE 反向视角:
  - SCN-01_VIEW-03_INSIDE_TO_OUT 门内看门外:
  - SCN-01_VIEW-04_OUTSIDE_TO_IN 门外看门内:

## 关键道具资产
### PRP-01 道具名
- 剧情功能:
- 造型锁定:
- 材质 / 标记 / 尺寸:
- 连续性要点:
- 生图提示词:
  - 45°侧视图:
  - 背视图:

## 缺口与假设
- ...
```

When the user wants only prompts, omit long analysis but keep asset IDs, scene continuity cards, and prompts. When the user wants a file artifact, save the result as Markdown and include a manifest mapping IDs to generated image filenames.

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
- no text labels, no UI, no watermark

Prop asset prompts must include:

- clean solid-color background
- `45°侧视图` prompt showing front/side form and important details
- `背视图` prompt showing rear construction and continuity marks
- material, scale, wear, mechanism, unique marks, and story-use details
- no hands unless the user asks for scale reference; no text labels, no watermark

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
- Character prompts include front view, back view, and headshot.
- Scene prompts are no-people location plates; master and derived views do not contradict each other.
- Prop prompts include 45° side view and back view.
- Prompts are clean, literal, and image-generation ready, not vague mood labels.
- Reference-image roles and conflicts are explicit when references exist.
- Guidance is prioritized, not bloated: preserve at most the highest-impact continuity constraints inside each final prompt.
- No invented prop, costume, scene, or spatial relation is presented as script fact; assumptions are labeled.
