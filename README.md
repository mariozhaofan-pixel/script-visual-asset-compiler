# 剧本资产提取器 / Script Visual Asset Compiler

`剧本资产提取器` 是一个面向影视、短剧、漫画、AI 视频和概念设计工作流的视觉资产提取系统。它把用户提供的剧本、故事梗概、导演拆解、分镜或混合资料，整理为可复用的角色、服装、场景、关键道具、电影感 LOOK 和生图提示词。

English summary: this repository contains a Codex Skill and GPT Builder knowledge-base versions for extracting production-ready visual asset plans and cinematic image prompts from scripts.

## 能力范围

- 从剧本中提取角色形象、服装状态、场景、关键道具和剧情功能。
- 为角色输出正视图、背视图、大头照的干净纯色背景生图提示词。
- 为场景输出无人全景母版，并在剧情需要时输出同一空间的多角度视图。
- 处理门内 / 门外、走廊 / 房间、正反打、路径和阈值空间的连续性。
- 为关键道具输出纯色背景、45 度侧视图和背视图提示词。
- 支持电影感 LOOK、LOOK CARD、LOOK 迁移、场景空镜和带人物分镜帧提示词。
- 支持 GPT Image、Midjourney、Flux、SDXL 等生图模型的轻量适配。
- 保持稳定资产 ID，便于后续分镜、视频提示词和图片资产管理。

## 仓库内容

```text
skill/script-visual-asset-compiler/
  SKILL.md                  Codex Skill 主规则
  agents/openai.yaml        Skill 展示名和默认提示
  references/cinematic/     电影感 LOOK、参数、preset、防廉价和模型适配规则

gpt/knowledge/
  00_GPT创建配置.md
  01_主控执行协议.md
  02_资产提取规则.md
  03_场景连续性与多视角.md
  04_生图提示词规范.md
  05_输出模板.md
  06_质量闸与测试用例.md
  README_上传清单.md
  剧本资产提取器_GPT创建修改提示词.md

gpt/knowledge-lite/
  00_GPT轻量创建配置.md
  01_轻量主控协议.md
  02_电影感风格层.md
  03_防廉价与模型适配.md
  04_轻量输出模板.md
  README_上传清单.md
  剧本资产提取器_轻量版_GPT创建修改提示词.md

gpt/legacy-single-file/
  剧本资产提取器_GPT智能体单文件版.md

docs/
  gpt-builder-setup.md
```

## 安装为 Codex Skill

把 `skill/script-visual-asset-compiler` 复制到本机 Codex skills 目录：

```powershell
Copy-Item -Recurse -Force `
  ".\skill\script-visual-asset-compiler" `
  "$env:USERPROFILE\.codex\skills\script-visual-asset-compiler"
```

之后可以用 `$script-visual-asset-compiler` 调用。用户可见名称是 `剧本资产提取器`。

## 创建 GPT 智能体

1. 打开 GPT 创建器。
2. 按 `gpt/knowledge/README_上传清单.md` 上传知识库文件。
3. 把 `gpt/knowledge/剧本资产提取器_GPT创建修改提示词.md` 粘贴到 GPT 创建器对话框。
4. 以 `00_GPT创建配置.md` 和其他多文件知识库为最高优先级规则。

如果需要更轻、更快的 GPT 版本，改用 `gpt/knowledge-lite/README_上传清单.md` 和 `gpt/knowledge-lite/剧本资产提取器_轻量版_GPT创建修改提示词.md`。

更详细步骤见 [docs/gpt-builder-setup.md](docs/gpt-builder-setup.md)。

## 输出原则

- 默认输出中文、制作向、可直接用于生图。
- 不只总结剧情，而是建立可复用视觉资产系统。
- 场景资产必须是无人环境图，角色出现在镜头画面时应另建 `SHOT-*` 或分镜帧，不混入 `SCN-*` 场景资产。
- 电影感 LOOK 是派生层，不改写剧本事实、资产 ID 或空间连续性。
- 生成图片时建议按依赖顺序：角色 / 服装、场景母版、场景衍生视角、道具、分镜帧。

## License

MIT License. See [LICENSE](LICENSE).
