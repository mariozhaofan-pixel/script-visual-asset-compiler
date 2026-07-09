# Generic Canvas Blueprint and CLI Execution

Use this file when the user asks for canvas nodes, infinite canvas planning, CLI node creation, LibTV, 小云雀, updream, neowow, or any other canvas tool with a command-line interface.

This skill must also work without a canvas tool. If no CLI execution is requested or available, output the asset prompts and optional blueprint only.

## Modes

Choose one mode:

1. `prompt_only`: default for Codex/GPT chat when the user only needs prompts.
2. `blueprint_only`: output a canvas plan and node content, but do not run a CLI.
3. `cli_command_plan`: output commands or a JSON payload for a canvas CLI, but do not execute.
4. `execute_cli`: run a detected/provided canvas CLI only when the user explicitly asks to create/update canvas nodes.

In GPT Builder or any environment without tool execution, always use `prompt_only`, `blueprint_only`, or `cli_command_plan`.

## Secret Safety

Never read, print, modify, or write API keys or access tokens.

Allowed:

- check whether an executable path exists
- run `--help`, `help`, or `version` commands
- use already-authenticated CLI commands
- pass non-secret prompt/node payloads

Not allowed:

- print environment variables that may contain keys
- read config files to inspect tokens
- run login flows without user request
- write credentials into any file
- include access keys in canvas node text, prompt docs, command logs, or final replies

## Canvas Tool Profile

Before execution, build a tool profile:

```yaml
canvas_tool:
  name:
  executable:
  working_directory:
  supports_coordinates:
  supports_sections_or_groups:
  supports_markdown_nodes:
  supports_json_import:
  create_session_command:
  create_node_command:
  update_node_command:
  dry_run_command:
  auth_assumption: already_configured
```

If the user names a tool but no command syntax is known, inspect help output only. If help is insufficient, stop at `cli_command_plan` and ask for the tool's node-creation syntax.

## Canvas Blueprint

Generate a Canvas Blueprint before creating nodes unless the user explicitly asks for prompt-only output.

Default sections:

```text
01_PROJECT_INDEX      project overview and usage order
02_STYLE_DNA          profile, color, material, lighting, model rules
03_CHARACTERS         CHR / CST assets
04_WORLD_ASSETS       SCN / PRP / CRE / VEH assets
05_STORYBOARD_VIDEO   STB / SHOT / VID assets
99_ARCHIVE            failed, deprecated, retry, scratch nodes
```

Recommended layout:

- Left: project index and style DNA.
- Center top: characters.
- Center bottom: world assets.
- Right: storyboard and video.
- Bottom/right: archive and retry nodes.

If the CLI supports coordinates, lay out sections from left to right. If it does not, preserve structure with section names, node names, metadata, tags, and an index table.

## Blueprint Format

```yaml
canvas_blueprint:
  project_name:
  profile:
  mode: prompt_only | blueprint_only | cli_command_plan | execute_cli
  layout_version: generic_canvas_blueprint_v1
  canvas_tool:
    name:
    executable:
  sections:
    - id: 01_PROJECT_INDEX
      purpose: project overview
      nodes:
        - node_name:
          node_type: note | prompt | generate | qa | ref | retry
          asset_ref:
          action:
          generate_now: false
          content_source:
    - id: 02_STYLE_DNA
      purpose: visual style, color, material, lighting
      nodes: []
    - id: 03_CHARACTERS
      purpose: character and costume assets
      nodes: []
    - id: 04_WORLD_ASSETS
      purpose: environments, props, creatures, vehicles
      nodes: []
    - id: 05_STORYBOARD_VIDEO
      purpose: storyboard, keyframes, video segments
      nodes: []
    - id: 99_ARCHIVE
      purpose: failed, deprecated, retry, scratch nodes
      nodes: []
```

## Node Naming

Use:

```text
{number}_{ASSETTYPE}_{asset_name}_{purpose}_v{version}
```

Examples:

```text
01_INDEX_ProjectOverview_v1
02_STYLE_VisualDNA_v1
03_CHARACTER_CHR-01_MainCharacter_PROMPT_v1
04_ENV_SCN-01_StorageRoom_PROMPT_v1
05_PROP_PRP-01_OldRadio_PROMPT_v1
06_CREATURE_CRE-01_MimicBehindWall_PROMPT_v1
07_STORYBOARD_STB-01_OpeningNineGrid_PROMPT_v1
```

For each asset group, keep related nodes close:

```text
PROMPT -> GENERATE -> QA -> REF(optional) -> V2_RETRY(optional)
```

Do not place all prompt nodes together and all generate nodes elsewhere.

## INDEX Node

Every canvas should include `01_INDEX_ProjectOverview_v1` with:

- project name
- one-sentence story
- project profile
- asset count
- section list
- node naming rule
- recommended usage order
- nodes requiring user confirmation before generation

## Execution Rules

- Do not execute image/video generation by default. Create prompt and generate nodes only.
- Only set `generate_now: true` when the user explicitly asks to generate immediately.
- If a canvas CLI creates empty nodes, retry by sending the full node content again.
- If nodes are scattered, reorganize with the same Blueprint instead of regenerating images.
- If CLI execution fails, return the prompt documents, Blueprint, and a clear failure point.

## Supported Tool Strategy

Known or user-mentioned CLI canvas tools may include LibTV, 小云雀, updream, neowow, or a custom local executable.

Do not hard-code one vendor. Use the following strategy:

1. Use the executable path explicitly provided by the user.
2. Otherwise search only common project-local paths if the user asked for CLI execution.
3. Run help/version to infer supported commands.
4. Prefer JSON import or markdown node creation if supported.
5. If syntax remains unknown, output `cli_command_plan` instead of guessing.

For LibTV-like tools, a preconfigured executable can be used only if it already exists and the user explicitly asks to create canvas nodes. Do not install it and do not inspect its credential storage.
