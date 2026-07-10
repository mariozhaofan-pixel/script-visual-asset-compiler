# Style Amplification

Use this file when the user asks for stronger stylization, live-action film stills, 真人电影, 影视风格, 玄幻低饱和, "风格化不够", character casting plates, or a script contains abstract systems that should become visible design language, such as zodiac, MBTI, personality panels, reputation heat, divine ranking, destiny wheels, or social-media metrics.

This file fixes a common failure mode: the skill may correctly load cinematic parameters but then output prompts that are too safe, flat, or concept-art-like. Cinematic style must become visible in the generated image through production design, lighting, palette, costume construction, materials, set behavior, and recurring motifs.

## Style Strength

Assign `style_strength` before compiling prompts:

- `base`: realistic, clean, restrained. Use when the user only asks for usable reference assets.
- `elevated`: clearly cinematic. Use when the user asks for movie stills, cinematic quality, high-end drama, or a named LOOK.
- `signature`: strongly stylized but still controlled. Use when the user says 风格化, 玄幻, 低饱和, 不够明显, 更有风格, or asks for a project visual bible.

For `signature`, every prompt needs at least three visible style carriers:

1. palette and lighting behavior
2. material system
3. symbolic or spatial motif
4. costume construction / set construction
5. camera and lens medium anchor

Do not rely on mood adjectives alone.

## Live-Action Fantasy Priority

When the user asks for 真人电影 or 影视风格, the prompt must include:

- `live-action fantasy drama still` or `cinematic live-action film still`
- real actor / practical costume / practical set photography language
- production design, costume department, prop department, makeup and hair realism
- natural skin pores, real fabric weave, metal edge wear, jade/stone/wood surface imperfections
- motivated light source and realistic falloff
- low saturation or specified palette behavior

Actively suppress the wrong medium:

```text
avoid illustration, anime, manga, game render, digital matte painting, plastic CGI, toy-like 3D, over-polished beauty filter, generic concept art
```

For scene plates, also suppress:

```text
no UI screens, no readable text, no floating labels, no game menu, no concept-art presentation board
```

## Cinematic Casting Plate

Use this route for `CHR-*` / `CST-*` when the user asks for styled front full-body images, 真人电影角色图, 定妆照, or a single character view.

Keep:

- one character only
- full-body front view or the exact requested view
- stable identity, costume, body proportion, and ID
- no extra characters, no readable text, no logos
- low-key plain studio, minimal cyclorama, or simple atmospheric stage background

Allow:

- cinematic lighting and palette
- subtle ground plane or studio shadow
- actor casting realism
- practical costume construction
- visible project DNA in costume cut, motifs, materials, halo, jewelry, armor seams, embroidery, makeup, or aura

Do not turn a casting plate into an action frame. No fighting pose, dramatic story scene, crowd, random set dressing, or unrelated prop unless the user asks.

Prompt skeleton:

```text
<CHR-ID> <name>, cinematic casting plate, full-body front view, one real actor only, <identity and temperament>.
Costume: <CST-ID, silhouette, layers, construction, material, wear, symbolic motifs>.
Style DNA: <palette>, <lighting>, <visual-system motif>, <project material system>.
Medium: live-action fantasy drama still, practical costume department, real skin/hair/fabric/metal/jade texture, <camera/lens if useful>.
Background: low-key plain studio or minimal stage background, no readable text.
Avoid: illustration, anime, game render, plastic CGI, over-beautified skin, extra people, watermark, logo, cropped feet.
```

## Scene Plate Amplification

For `SCN-*`, keep the scene empty but make the style visible through set construction:

- compile the final prompt body with `scene-shot-prompt-grammar.md` when the user asks for stylized scene images, cinematic scene plates, or unified scene prompt format
- use location plate / live-action set photography language
- define foreground, midground, background, entrances/exits, and fixed anchors
- specify practical materials and age: wet stone, worn jade, tarnished bronze, incense soot, carved wood, frost, silk, lacquer, paper
- define a motivated light source: moonlight, overcast skylight, candle/fire, fluorescent-celestial lamps, reflected water-mirror light
- state the palette as behavior, not only color names
- replace UI/text with abstract non-symbolic light, geometry, reflections, or spatial motifs

If the result looks like game CG or concept art, rerun with:

```text
real location set photography, practical full-scale set, no digital matte painting, no concept art, no game environment, no UI panels, no readable glyphs, tactile weathered materials, natural lens perspective
```

For scene and storyboard style references that use a structure like shot type, subject/action, environment, fixed composition, immutable style keywords, and negative keywords, preserve that order as a prompt-body grammar while keeping all existing asset IDs and continuity cards. Do not import sample subjects or example settings from the reference into the user's script.

## Visualizing Abstract Systems

Never leave zodiac, MBTI, reputation heat, or divine ranking as text labels. Convert them into non-readable visual grammar.

### Zodiac / Twelve-Sign Systems

Use:

- twelve-segment celestial wheel
- star-map ring with twelve empty chambers
- orbital jewelry, halo, floor inlay, ceiling constellation, or ritual compass
- element-coded material hints: fire ember, water moon-glass, earth jade/stone, air silk/cloud-metal
- birth-chart geometry, not text

Avoid:

- zodiac names, horoscope labels, readable star-sign text, UI cards

### MBTI / Sixteen-Type Systems

Use:

- 4-by-4 geometric grid
- sixteen unmarked crystal panes, mirrors, masks, seals, or personality shards
- four-axis split motifs: introversion/extraversion as inward/outward light, intuition/sensing as mist/texture, thinking/feeling as metal/silk, judging/perceiving as rigid/asymmetric structure
- cognitive-function orbits and fractured aura, without letters

Avoid:

- readable MBTI letters, type names, personality test UI, chart labels

### Reputation Heat / Public Opinion

Use:

- red-gold heat shimmer
- ink-black smoke from extinguished incense
- non-symbolic red/blue waveforms
- crowd energy as reflected light only when scene must stay empty
- cracked divine halo, polluted aura, brittle gold leaf, broken mirror-water

Avoid:

- social-media UI, readable comments, platform logos, charts with text

## Style Repair Checklist

Before finalizing a prompt for a style-heavy request, check:

- The prompt states `style_strength`.
- The medium is explicit: live-action film still / practical set / real actor / location plate.
- At least three concrete style carriers are present.
- Abstract systems are visualized as motifs, not labels.
- Character casting plates are not mistaken for action shots.
- Scene plates remain empty and spatially readable.
- Negative constraints target the actual failure mode: concept art, game render, plastic CGI, anime, readable text, UI pollution.
