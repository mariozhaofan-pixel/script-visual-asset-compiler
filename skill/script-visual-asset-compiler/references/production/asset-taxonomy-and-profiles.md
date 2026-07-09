# Asset Taxonomy and Project Profiles

Use this file when a script needs broader production planning beyond characters, scenes, and props, or when the user asks for a project profile, visual DNA, creature, vehicle, color palette, storyboard, video segment, or canvas workflow.

## Asset Taxonomy

Use stable IDs. Prefer the hyphen style used by this skill; accept underscore IDs from imported materials when preserving an existing project.

| Asset | ID | Purpose | Default priority | Prompt surface |
|---|---|---|---|---|
| Character | `CHR-01` | named or important people, roles, extras with continuity value | P0/P1 | identity, costume states, front/back/head |
| Costume State | `CST-01` | reusable clothing or state for a character | P0/P1 | clean costume-locked views |
| Creature | `CRE-01` | monster, animal, mechanical beast, mount, mutated life, non-human entity | P0/P1 | multi-view design sheet |
| Scene / Environment | `SCN-01` | reusable spatial location, set, room, street, battlefield, interior/exterior | P0/P1 | master plate and derived views |
| Prop | `PRP-01` | plot object, weapon, clue, device, token, product, ritual object | P1/P2 | 45-degree side and back, optional detail |
| Vehicle | `VEH-01` | car, ship, aircraft, mecha, spacecraft, bike, cart, boat | P1/P2 | side/front/top/3-4 views |
| Color / Material DNA | `DNA-01` | palette, material system, lighting rules, visual grammar | P0 | flat reference sheet or text spec |
| Storyboard / Keyframe | `STB-01` / `SHOT-*` | beat grid, keyframe, cinematic still, storyboard panel | P1/P2 | frames referencing asset IDs |
| Video Segment | `VID-01` | image-to-video or text-to-video segment plan | P2 | style lock, action timeline, continuity |

Priority:

- `P0`: must be created first or later continuity will break.
- `P1`: core story asset.
- `P2`: world-building or supporting asset.
- `P3`: optional details, variants, textures, later refinements.

## Extraction Requirements

Every extracted asset should include:

```yaml
asset_id:
asset_name:
asset_type:
asset_role:
priority:
story_function:
template_surface:
required_views:
aspect_ratio:
dependencies:
prompt_status: draft
qa_risk:
```

Dependencies matter. For example, `SHOT-01_FRAME-01` should reference `CHR-*`, `CST-*`, `SCN-*`, and `PRP-*` instead of restating new facts.

## Project Profile

Create or select a project profile before compiling prompts when the script has a distinct world, genre, or visual system.

Profile selection priority:

1. User-specified profile.
2. Closest existing profile by genre and visual DNA.
3. Temporary profile inferred from the script.
4. Generic cinematic profile.

Profile fields:

```yaml
profile_id:
project_name:
profile_type:
language:
visual_dna:
  world:
  references:
  materials:
  colors:
lighting:
  asset_sheet:
  environment:
style_constraints:
  avoid:
qa_focus:
```

## Reusable Profile Patterns

Use these as starting points, not fixed projects.

### Generic Cinematic

- World: infer from script.
- Look: realistic cinematic still, natural lens behavior, restrained color grading.
- Materials: script-defined.
- Avoid: generic AI look, oversharpening, plastic skin, random costume changes, inconsistent characters.
- QA: character consistency, scene usability, prop function, storyboard continuity.

### Historical Court / Ancient Drama

- World: court politics, wood architecture, silk costume, candlelight, restrained period drama realism.
- Materials: silk, embroidery, carved wood, paper windows, bronze, rain-wet stone.
- Colors: candle gold, dark wood, desaturated red, jade green, smoke grey.
- Avoid: beauty-filter plastic skin, cheap cosplay, over-bright fantasy glow.

### Confined Apocalypse Thriller

- World: enclosed space, survival suspense, old devices, dust, emergency light, unknown threat.
- Materials: concrete, rusted metal, cracked paint, plastic emergency lights, old radio.
- Colors: cold grey, emergency red, dirty white, rust brown, sickly green.
- Avoid: generic zombie cliche unless requested, over-gore, clean sci-fi lab, heroic action look.

### Cyber City

- World: dense city, neon rain, street tech, underclass alleys, mixed poverty and technology.
- Materials: wet asphalt, glass, LED signage, scratched plastic, black tech fabric.
- Colors: cyan neon, magenta neon, sodium orange, deep black, rain grey.
- Avoid: clean luxury sci-fi, cartoon neon, empty streets unless requested.

## Color and Material DNA

Create `DNA-01` when:

- multiple assets must share a world style
- the user asks for a visual bible, style guide, color card, or production bible
- scene and costume palettes need locking before generation

`DNA-*` can be text-only, a flat color/material sheet, or both. It should define:

- 5-8 color swatches with names and functions
- recurring materials
- lighting rules
- forbidden visual drift
- asset-specific inheritance notes

Do not treat a color card as a cinematic scene. It is a production reference.
