# Prompt Document Compiler

Use this file when the user asks for industrialized prompts, asset nodes, generated image/video prompt documents, or canvas-ready production notes.

## Layer Separation

Keep three layers separate:

1. `Prompt Body`: the exact text intended for an image/video model.
2. `Parameters`: aspect ratio, model, quality, reference images, canvas section, node metadata.
3. `Production Notes`: QA checklist, rerun fixes, version suggestions, operator instructions.

Never put QA instructions, rerun notes, CLI commands, or canvas metadata inside the model prompt body.

## Standard Prompt Document

Use this structure for canvas-ready or file-ready assets:

```text
=== Image Prompt - {project_name} {episode_or_scene} {asset_id} {asset_name} {asset_type} v{version} ===

【格式】
- aspect_ratio:
- layout:
- view_count:
- quality:

【资产定位】
- story_function:
- visual_memory_point:
- dependencies:
- hierarchy_or_power_level:

---

【prompt】
<model prompt body only>
=== END ===

【参数建议】
- model:
- aspect_ratio:
- reference_images:
- canvas_section:
- create_generate_node:

【实操注意】
1. 跑出来重点检查：
   - ...
2. 跑歪兜底加强：
   - ...
3. 版本迭代建议：
   - ...
```

Prompt body order:

1. Opening format sentence.
2. Layout or view structure.
3. Panel/view details.
4. Subject.
5. Core identity or construction.
6. Appearance, costume, equipment, material, or environment.
7. Color palette.
8. Pose, camera, or view.
9. Critical consistency.
10. Lighting.
11. Style.
12. Quality.
13. Labels if needed.
14. Constraints.

## Asset Template Surfaces

### Character Reference

Use this when the user wants a production character sheet rather than the default separate front/back/head prompts.

- Aspect: 9:16.
- Layout: 2x2 uneven grid.
- Views: front face close-up, side face close-up, body front without face, full back including back of head.
- Requirement: same person, same outfit, same proportions, same lighting.
- Keep this as an optional sheet format; the default skill output still supports separate front, back, and headshot prompts.

### Creature / Mechanical Beast

- Aspect: 16:9.
- Layout: 2x2 equal grid.
- Views: side, front, top-down, 3/4 perspective.
- Include anatomy or construction, scale, surface material, signature visual memory point, limbs/body parts, functional story elements, damage/weathering.
- Requirement: same creature across all views.

### Environment

- Aspect: 16:9 by default.
- Layout: single master, 3-view spatial setting, or mood board only when requested.
- Include scene summary, spatial layout, architecture/set design, key story areas, atmosphere, color palette, camera/composition, lighting.
- Requirement: no people for `SCN-*` assets. If people appear, create `SHOT-*`.

### Prop / Weapon / Device

- Aspect: 16:9 or 1:1.
- Views: 45-degree side view and back view by default; add front/detail only when needed.
- Include form, structure, materials, functional parts, scale/handling, wear/history, color palette.
- Requirement: clean solid-color background unless user requests a production board.

### Vehicle

- Aspect: 16:9.
- Views: side, front, top, 3/4; optional cockpit, wheel, engine, control detail.
- Include silhouette, scale, movement system, cockpit/interface, materials, damage, decals/customization.
- Requirement: same vehicle across all views.

### Color / Material DNA

- Aspect: 16:9 if visual sheet.
- Layout: flat production design reference, not a cinematic scene.
- Include color swatches, names, functions, material DNA, lighting DNA, forbidden drift.
- Avoid photography, shadows, 3D mockups, watercolor, decorative poster styling.

### Storyboard Grid

- Aspect: 16:9 or 3:4.
- Layout: 3x3 nine-panel grid.
- All panels must belong to one sequence or clearly connected beats.
- Each panel should reference stable `CHR/CST/SCN/PRP` IDs and preserve lighting, palette, and spatial continuity.
- Use `STB-*` for a grid sheet and `SHOT-*` for individual frames.

### Video Segment

- Use `VID-*` only when the user asks for video planning or image-to-video prompts.
- Include style lock, scene, character lock, camera language, action timeline, emotion/performance, lighting, sound/music, continuity, negative/avoid.
- Do not start final video generation unless explicitly requested.

## Prompt Language

- Use Chinese for analysis, asset maps, QA, and user-facing explanations by default.
- Use English or bilingual model prompt bodies when precise visual control benefits from English.
- Preserve user-requested language.

## QA and Rerun

Each asset should have a QA checklist:

- identity and continuity
- view/layout compliance
- material and palette accuracy
- lighting/style inheritance
- labels only when required
- model-specific risk

Rerun rule:

```text
Do not rewrite the whole prompt first. Append a failure-specific reinforcement sentence and create v2.
```

Versioning:

- `v1`: first usable draft.
- `v2`: fixes main failure.
- `v3`: locks accepted references.
- `vFinal`: user-approved.
