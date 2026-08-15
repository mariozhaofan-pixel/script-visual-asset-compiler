# Character and Costume Workflow

Use this file when extracting characters, costume states, creature/person dependencies, or default character image prompts.

## Dependency Graph

Create a dependency graph before writing final character/costume prompts when the project has families, multiple forms, transformations, age states, injuries, or major costume changes.

Required directed dependencies:

- blood relatives;
- the same person's costume, age, injured, wet, dirty, ceremonial, disguise, or transformed states;
- twins, clones, host/form pairs, armor-bound or creature-bound forms;
- continuous damage or mutation stages.

Default dependency order:

```text
independent base identity -> family/derived identity -> costume states -> injury/age/transformation states -> shot frames
```

If a downstream asset depends on an unconfirmed upstream image or identity, output a waiting card instead of inventing the downstream prompt.

## Production List

Before writing large batches of prompts, output a production list for user confirmation:

| ID | 角色/状态 | 资产类型 | 上游依赖 | 建议批次 | 必要性 | 出图理由 | 当前决定 |
|---|---|---|---|---|---|---|---|
| CHR-01 / CST-01 | ... | 基础角色/服装/亲属衍生/变异/受伤/年龄 | 无/CHR-xx/CST-xx | 第1批 | 必须/建议/文字足够 | ... | 待确认 |

Do not over-split tiny expression changes, ordinary held objects, or dirt that can be handled in a later shot prompt.

## Current Default: Costume Design Sheet

The default character image deliverable is no longer front/back/headshot. Output a `CST-*` costume design sheet for each necessary costume state.

Keep two layers:

- `CHR-*`: identity lock in text, covering age range, body proportion, facial/hair constraints, temperament, class, posture, relationship function, and performance baseline.
- `CST-*`: image prompt for the actual costume design sheet, bound to one `CHR-*`.

Only output actor front view, back view, headshot, or three-view prompts when the user explicitly asks for them.

## Costume Design Sheet Prompt Template

Use this as the default visual prompt for a character/costume asset:

```text
生成一张专业影视角色服装设计图，资产ID: CST-xx，绑定角色: CHR-xx [角色名]。画面为干净纯色/浅灰无缝影棚背景，单一角色服装资产，不是剧情剧照，不出现场景杂物。

版式: 一张服装设计图，优先展示服装信息。左侧为严格正面全身服装视图，可使用无脸中性模特或从颈根以下展示以避免脸部漂移；右侧为严格背面全身服装视图，完整展示后背、后摆、发型/头饰背部结构（如该信息影响服装）；下方或侧边加入2-4个局部细节小窗，展示面料纹理、刺绣/纹样、腰封/甲片/纽扣/金属件/鞋履/随身配饰。所有视图必须是同一体型、同一服装、同一比例，左右非对称锚点方向一致。

角色适配: [年龄段、身高体态、职业/阶层、气质、姿态]。服装设计: [逐件写轮廓、层级、剪裁、颜色、材质、磨损、污渍、破损、宗教/星座/MBTI/家族/职业等无文字视觉语法、关键配饰]。材质必须符合真实制衣和电影服化道工艺，不像廉价cosplay，不像游戏皮肤。

最高优先级: 真实肌肤质感/真实材质质感, 真实光影效果和阴影高光, 统一光线来源, 遵从真实光线; 不要展示全部细节, 暗部干净无噪点, 让部分信息自然沉入阴影。使用柔和但有方向的中性棚拍主光，保留自然接触阴影，服装固有色准确，金属/玉石/皮革/丝绸/棉麻按真实材质反光。

真人电影服装设计图，真实演员比例或专业服装展示模特，实际可制作服装，低饱和电影调色，清晰可读的服装结构。限制: 纯色干净背景, 单一角色服装资产, 无额外人物, 无剧情动作, 无场景, 无文字标签, 无水印, 无logo, 不改变CHR-xx身份/体型/服装状态/左右锚点。
```

When the user wants a simpler output, compress the template but preserve: ID, bound `CHR-*`, front/back costume layout, key detail windows, material realism, single light source, no text/watermark, and no extra people.

## Optional Actor/Casting Views

If explicitly requested, use stable view names:

- `CHR-xx_FRONT`: full-body front view.
- `CHR-xx_BACK`: full-body back view.
- `CHR-xx_HEAD`: head-and-shoulders portrait.

These are optional actor/casting assets. They do not replace `CST-*` costume design sheets.

## Acceptance Rules

Check every costume sheet for:

- correct `CHR-*` binding and `CST-*` state;
- same body proportion across front/back/details;
- complete clothing silhouette and shoes/hem/sleeves;
- correct left/right asymmetric anchors;
- fabric, metal, leather, jade, wood, dirt, wetness, damage, or embroidery behavior;
- no unintended face drift if a face is visible;
- no readable labels unless the user explicitly requests a labeled production board.
