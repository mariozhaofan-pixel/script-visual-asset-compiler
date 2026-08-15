# Codex Image Generation

Use this file only in Codex or another local execution environment when the user asks to generate image files from compiled asset prompts, especially with `image_gen.py`, `gpt-image-2`, 4K PNG output, or ID-mapped output paths.

Do not load this file for GPT Builder behavior. GPT versions should output prompts, prompt documents, and optional canvas blueprints. They should not output local execution commands unless the user explicitly asks for Codex instructions.

## Execution Boundary

- Generate files only when the user asks to generate images, create image outputs, or run the local image tool.
- Do not create, install, or modify `image_gen.py` unless the user explicitly asks.
- Use `image_gen.py` only when it exists in the current workspace or the user provides a valid path.
- Do not read, print, write, or embed API keys in commands, prompts, output paths, logs, or generated documents.
- Send only the final model-facing prompt body to `--prompt`. Do not include QA notes, extraction analysis, canvas metadata, rerun instructions, or CLI comments inside the prompt.

## Default 4K Portrait Command

When `image_gen.py` is available and the user wants a 4K portrait PNG, use this default:

```powershell
python image_gen.py generate --model gpt-image-2 --prompt "<final prompt body only>" --size 2160x3840 --quality high --output-format png --out output/imagegen/<asset_id>_<view>.png
```

PowerShell multiline form:

```powershell
python image_gen.py generate `
  --model gpt-image-2 `
  --prompt "<final prompt body only>" `
  --size 2160x3840 `
  --quality high `
  --output-format png `
  --out output/imagegen/<asset_id>_<view>.png
```

POSIX shell form:

```bash
python image_gen.py generate \
  --model gpt-image-2 \
  --prompt "<final prompt body only>" \
  --size 2160x3840 \
  --quality high \
  --output-format png \
  --out output/imagegen/<asset_id>_<view>.png
```

Use `2160x3840` as the default 9:16 4K portrait size when the user asks for 4K vertical, phone-screen, short-drama, poster, or unspecified high-resolution portrait output. If the user names another aspect ratio or the tool documents another supported size, adapt the size only to match that request.

## Output Naming

Create deterministic, ID-mapped paths under `output/imagegen/`:

```text
output/imagegen/CST-01_design.png
output/imagegen/SCN-01_MASTER.png
output/imagegen/SCN-01_VIEW-03_INSIDE_TO_OUT.png
output/imagegen/SCN-01_VIEW-04_OUTSIDE_TO_IN.png
output/imagegen/PRP-01_45deg.png
output/imagegen/PRP-01_back.png
```

Keep file names ASCII-safe when possible. Preserve the asset ID and view name even if the display title is Chinese.

## Batch Order

Generate in dependency order:

1. `DNA-*` visual DNA or palette lock, if requested.
2. `CHR-*` identity text locks and `CST-*` costume design plates.
3. `CRE-*` creatures and `VEH-*` vehicles when present.
4. `SCN-xx_MASTER` scene master plates.
5. `SCN-xx_VIEW-*` derived scene views, using the accepted master scene plate as geometry/style reference when the tool supports references.
6. `PRP-*` plot props.
7. `SHOT-*` storyboard/keyframe images only when the user asks for character-containing frames.

For each generated file, record:

```text
asset_id | view | prompt_source | output_path | status | notes
```

## Prompt Preparation

Before running a command, compress the prompt to the final image-generator text:

- Start with asset ID, subject, and view.
- Preserve identity, costume, scene anchors, prop form anchors, and spatial continuity constraints.
- Include clean-background constraints for costume, optional character actor views, creature, vehicle, and prop reference plates.
- Include no-people constraints for `SCN-*` location plates.
- Omit production notes, markdown headings, analysis, and canvas node metadata.
- Escape quotes or use shell-safe quoting so the command passes the full prompt unchanged.

If a prompt is too long for reliable shell execution, shorten decorative language first. Do not remove ID locks, geometry anchors, wardrobe identity, prop structure, or no-people / clean-background constraints.

## Reporting

After generation, report only the useful result map:

```text
CST-01_design -> output/imagegen/CST-01_design.png
SCN-01_MASTER -> output/imagegen/SCN-01_MASTER.png
PRP-01_45deg -> output/imagegen/PRP-01_45deg.png
```

Mention any failed command with the error class and the asset ID. Do not print secrets, full environment dumps, or unrelated command output.
