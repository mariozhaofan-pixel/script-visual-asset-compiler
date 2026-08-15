# Emotion Curve

Use this file when the user asks for an emotional curve, rhythm map, tension graph, act-level intensity, BGM emphasis, or a director overview image/prompt.

## Purpose

The emotion curve shows the audience's tension / emotional impact across the whole story. It is a director planning artifact, not a scene asset, prop, or storyboard replacement.

## Calculation Rules

- Use one combined curve from `0.0` to `10.0`.
- Choose nodes from real dramatic change: external threat, decision, failure, reveal, physical danger, relationship reversal, false safety, release, and aftershock.
- Use 8-14 nodes by default, 16 maximum. Merge adjacent beats with the same direction when the story is dense.
- Number nodes in story order. Each node has a short name and one-decimal intensity.
- Mark act zones, highest peak, major reversal, truth reveal, breathing zone, BGM emphasis, and ending aftershock when present.
- Do not force the opening or ending to zero.
- Keep at least one breathing zone only when the script actually provides a release or temporary safety beat.
- BGM emphasis reuses existing curve node IDs and names. Do not invent separate music-only events.

## Text Output

For chat-only output, use this compact table:

```markdown
## 全片情绪曲线
| 节点 | 剧情节拍 | 强度 | 变化原因 | 标记 |
|---|---|---:|---|---|
| 1 | ... | 2.0 | ... | 建立 |

- 最高峰:
- 关键反转:
- 呼吸区:
- BGM重点:
- 全片原则:
```

## Image Prompt

If the user wants the curve image, create a 16:9 director diagram prompt:

```text
《项目名》全片情绪曲线图, 16:9 horizontal cinematic director planning infographic, matte black / deep charcoal background, subtle paper or film grain, low-contrast grey grid, one clean tension curve from cool dark grey-blue through muted amber to deep dark red at the peak. Y axis labeled 情绪强度 with marks 0 2 4 6 8 10 and small labels 平静 / 不安 / 窒息; X axis divided by actual act structure. Each node displays 编号. 短名称 数值, highest peak and key reversal have one short annotation. Top-right legend: 建立 收紧 升级 窒息 释放 余震. Bottom lines: 呼吸区, BGM重点, 全片原则. Clear Chinese typography, generous safe margins, no overlapping labels, no characters, no scene illustration, no photo, no logo, no watermark.
```

If image generation fails or is unavailable, output the table and prompt; do not block asset extraction.
