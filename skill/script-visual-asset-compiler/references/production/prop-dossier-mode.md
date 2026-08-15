# Prop Dossier Mode

Use this file when filtering key props or when the user asks for a key prop mother plate / product dossier.

## Candidate Filter

Extract a prop only when it has at least one strong production reason:

- drives plot, threat, reveal, relationship, status, or handoff;
- repeats across scenes;
- is grabbed, attacked with, opened, broken, hidden, exchanged, or recognized;
- contains a mark, inscription, missing corner, pattern, mechanism, direction, damage, or material feature that affects continuity;
- would be visually ambiguous if left as text only.

Use a candidate table before large batches:

| ID | 道具 | 剧情作用 | 识别/连续性锚点 | 必要性 | 出图理由 | 当前决定 |
|---|---|---|---|---|---|---|
| PRP-01 | ... | ... | ... | 必须/建议/文字足够 | ... | 待确认 |

Do not turn ordinary furniture or one-time clutter into prop assets unless it affects blocking, recognition, continuity, or story reversal.

## Default View Mode

The main skill default remains:

- `45°侧视图`: clean solid-color background, showing front/side form and important details.
- `背视图`: clean solid-color background, showing rear construction, interfaces, marks, and wear.

Use this when the user wants reusable prompt components, design iteration, or separate reference views.

## Single Product-Dossier Mode

Use a single 3:4 product-dossier prompt when the user asks for 道具母板, 产品档案照, cleaner prop cards, fewer views, or when the prop is best understood as one complete object.

```text
生成一张专业影视道具母板，只展示 PRP-xx [道具名称] 这一件物体。画面严格使用3:4竖构图，纯白或浅灰无缝影棚背景，高调但有方向的柔和中性光，边缘清晰，只保留轻微自然接触阴影。采用单件高端产品档案摄影形式，不同时展示多个角度。

构图: [长条形道具采用左下到右上干净对角线；紧凑道具居中略偏并保留呼吸空间]，主体占画面65%-80%，完整展示全部结构，不裁切[关键功能部位/握持端/刃口/开合机构/文字或图案区域]。

结构与材质: [真实尺寸与比例]，[主体结构、连接方式、材质、固有色、表面工艺、磨损、左右非对称、文字或图案方向、可抓握部位、剧情锁定状态]。所有结构符合真实制造工艺与物理受力，材质呈真实哑光或受控反射，不做塑料化高光。

最高优先级: 真实材质质感, 真实光影效果和阴影高光, 统一光线来源, 遵从真实光线; 不要展示全部细节, 暗部干净无噪点, 让部分信息自然沉入阴影。真人实物电影道具摄影质感。限制: 单件道具, 无人物, 无手, 无场景杂物, 无额外配件, 无说明文字, 无标签, 无水印, 无logo, 非CG, 非游戏装备, 不改变材质/标记/比例。
```

If a real reference image exists, state exactly what it controls. Never invent `{{Image 1}}` or `{{Image 2}}` placeholders when no uploaded image exists.

## QA

Check: single object, correct aspect, complete functional parts, believable scale, manufacturable structure, grip/contact points, era-appropriate material, correct marks/damage/direction, no hand/person/extra object, no accidental brand or readable label unless required by the script.
