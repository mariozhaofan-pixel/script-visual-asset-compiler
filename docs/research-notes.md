# Research Notes

This repository does not copy code, assets, datasets, model weights, or prompts from the referenced projects. The projects below were reviewed as design references for structuring a script-to-visual-assets workflow.

## Projects Reviewed

- Story2Board: https://github.com/daviddinkevich/Story2Board
  - Useful for thinking about story-to-storyboard segmentation and the difference between scene beats and generated frames.
- StoryDiffusion: https://github.com/HVision-NKU/StoryDiffusion
  - Useful for character consistency concerns across generated story images.
- SDeC: https://github.com/tntek/SDeC
  - Useful as a reference point for script decomposition and structured creative pipelines.
- OpenClap-Format: https://github.com/jbilcke-hf/OpenClap-Format
  - Useful for separating creative content into reusable, machine-readable production fields.
- Jellyfish: https://github.com/Forget-C/Jellyfish
  - Useful for understanding multimodal continuity and long-context visual reasoning concerns.
- ViMax: https://github.com/HKUDS/ViMax
  - Useful for thinking about visual consistency, reference conditioning, and asset reuse across later generation stages.

## Design Decisions Adopted

- Keep source truth, continuity context, derived prompts, and image-generation payloads separate.
- Use stable IDs for characters, costumes, scenes, scene views, props, and optional shot frames.
- Treat scene assets as reusable empty location plates, not character-containing cinematic frames.
- Generate scene master plates before derived angles.
- Add derived scene views only when the story needs continuity across thresholds, reverse angles, entrances, exits, chases, searches, reveals, or repeated locations.
- Keep GPT Builder rules modular so the GPT can retrieve the relevant rule file instead of relying on one large instruction block.

## Third-Party Code Policy

No third-party source files are included in this repository. If future contributors add adapters, examples, or code inspired by an external project, they should preserve upstream licenses and place attribution beside the contributed material.
