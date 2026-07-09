# LOOK Card and Transfer

Use this file for fuzzy style requests, named-look transfer, and cinematic variants.

## Request Routes

When the user asks for stronger stylization, 真人电影, 影视风格, 玄幻低饱和, zodiac/MBTI visualization, or says the result is not stylized enough, load `style-amplification.md` before assembling final prompts.

### Exact Preset

If the user names a known preset, show, film, DP-style, or exact look from `presets-reference.md`:

1. Load the matching preset.
2. Use its LOOK layer directly.
3. Replace only the subject, scene, asset view, or frame content with the user's script-derived content.
4. Do not output a LOOK CARD unless the user asks to compare options.

### Fuzzy Look

If the user gives only a vague mood or genre, such as "高级剧集感", "赛博朋克夜景", "冷峻电影感", or "浪漫胶片感":

1. Build one candidate LOOK from `recipes-reference.md`.
2. Select one `CAMERA_KIT`, one `LIGHT_RECIPE`, one `COLOR_GRADE`, and one mood.
3. Output a LOOK CARD and wait for confirmation before writing final full prompts, unless the user explicitly asks to continue automatically.

LOOK CARD format:

```text
LOOK CARD — <题材/情绪描述>
--------------------------------
CAMERA_KIT   <id>（名称） — <选择原因>
LIGHT_RECIPE <id>（名称） — <选择原因>
COLOR_GRADE  <id>（名称） — <选择原因>
MOOD         <mood>      — <选择原因>
STYLE_STRENGTH <base/elevated/signature> — <选择原因>
VISUAL MOTIFS <3-5 recurring motifs, no readable text>
适用资产     <SCN/SHOT/CHR/PRP 使用边界>
--------------------------------
确认后我会把该 LOOK 应用到资产提示词；也可以调整任一类。
```

### LOOK Transfer

If the user asks to apply one look to another script, scene, asset, or shot:

- Transfer: lighting, color/texture, mood, camera/lens feel, anti-slop bucket.
- Replace: story facts, character identity, scene layout, prop form, asset view requirements.
- Preserve: clean background for characters and props, no-people rule for scene plates, master/derived scene continuity.

### Variants

If the user asks for several cinematic directions for the same asset package:

1. Keep extracted asset IDs and script truth unchanged.
2. Vary only the LOOK layer and, for SHOT frames, dramatic moment / shot size / composition / angle.
3. Output a compact comparison table first unless the user says "都要".
4. If the user says "都要", output one prompt set per variant.

## Prompt Layer Order for SHOT Frames

For character-containing cinematic frames, assemble each prompt in this order:

```text
L1 [SCENE] Subject + action or environment moment.
L2 [FRAMING] shot + aspect + angle + composition.
L3 [FOREGROUND] foreground depth element; skip if foreground is none.
L4 [CAMERA] camera body + lens + aperture.
L5 [LIGHTING] light_style + light_source + fill.
L6 [COLOR/TEXTURE] palette + saturation + film/color behavior + grain/texture + halation.
L7 [MOOD] emotional atmosphere.
L8 [REALISM] subject-aware realism + photography anchors + clarity + tonal density.
L9 [ANTI-SLOP] Tier A + one Tier B clause, adapted to target model.
L10 [MODEL NOTE] Optional model-specific note.
```

For SCN assets, omit acting and keep the environment empty. For default CHR/CST/PRP reference plates, use only asset-safe realism and anti-slop. For cinematic casting plates requested by the user, use `style-amplification.md`: keep single-subject/full-view constraints, but allow live-action film medium anchors, practical costume/set language, cinematic palette, lighting, and visible project motifs.

## Subject Priority

If two or more subjects appear in a SHOT frame, declare the visual center:

```text
视觉中心是 <主体ID/主体名>，其他人物与物件只作为叙事衬托。
```

For subjectless scenes, describe material, weather, time, environmental behavior, and spatial anchors instead of inventing a character.
