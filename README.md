# 剧本资产提取器 / Script Visual Asset Compiler

`剧本资产提取器` 是一个面向影视、短剧、漫画、AI 视频和概念设计工作流的视觉资产提取系统。它把用户提供的剧本、故事梗概、导演拆解、分镜或混合资料，整理为可复用的角色、服装、场景、关键道具、生物/怪兽、载具、色卡/视觉DNA、电影感 LOOK、工业化提示词文档和通用画布蓝图。

English summary: this repository contains a Codex Skill and GPT Builder knowledge-base versions for extracting production-ready visual asset plans, prompt documents, cinematic image prompts, and optional canvas blueprints from scripts.

## 能力范围

- 从剧本中提取角色形象、服装状态、场景、关键道具和剧情功能。
- 为角色输出正视图、背视图、大头照的干净纯色背景生图提示词；用户要求时可改为真人电影定妆照式单视图资产。
- 为场景输出无人全景母版，并在剧情需要时输出同一空间的多角度视图。
- 处理门内 / 门外、走廊 / 房间、正反打、路径和阈值空间的连续性。
- 为关键道具输出纯色背景、45 度侧视图和背视图提示词。
- 支持生物/怪兽、载具、色卡/材质DNA、分镜网格和视频段落计划。
- 支持工业化提示词文档：Prompt Body、参数建议、实操注意、跑歪兜底加强分层。
- 支持电影感 LOOK、LOOK CARD、LOOK 迁移、场景空镜和带人物分镜帧提示词。
- 支持 GPT Image、Midjourney、Flux、SDXL 等生图模型的轻量适配。
- Codex Skill 可规划通用 Canvas Blueprint，并在用户明确要求且本地已有安全 CLI 时创建节点；GPT 版本只输出通用画布蓝图，不输出 CLI 命令计划。
- 保持稳定资产 ID，便于后续分镜、视频提示词和图片资产管理。

## 仓库内容

```text
skill/script-visual-asset-compiler/
  SKILL.md                  Codex Skill 主规则
  agents/openai.yaml        Skill 展示名和默认提示
  references/cinematic/     电影感 LOOK、参数、preset、防廉价和模型适配规则
  references/production/    资产分类、Project Profile、工业化提示词文档规则
  references/canvas/        通用 Canvas Blueprint 与 Codex CLI 执行安全规则

gpt/knowledge/
  00_GPT创建配置.md
  01_主控执行协议.md
  02_资产提取规则.md
  03_场景连续性与多视角.md
  04_生图提示词规范.md
  05_输出模板.md
  06_质量闸与测试用例.md
  07_场景分镜提示词语法.md
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
- 风格化请求会进入强化编译层：真人电影、玄幻低饱和、星座/MBTI 等抽象系统需要变成可见的服化道、场景结构、命盘/格栅/星轨等无文字视觉语法。
- 生图提示词最高优先级：真实肌肤/材质质感、真实光影和阴影高光、统一光线来源、遵从真实光线；不要展示全部细节，暗部干净无噪点。
- 场景图和分镜图使用统一提示词顺序：景别/视角、主体动作或无人物空间状态、环境、固定构图、高权重风格关键词、限制项。
- GPT 版本只输出提示词、提示词文档和通用画布蓝图，不输出 CLI 命令计划。
- Codex 本地环境需要实际生成图片时，可按规则调用已有 `image_gen.py`，默认使用 `gpt-image-2`、`2160x3840`、`high`、`png`，并输出到 `output/imagegen/<asset_id>_<view>.png`。
- 生成图片时建议按依赖顺序：视觉DNA、角色 / 服装、生物 / 载具、场景母版、场景衍生视角、道具、分镜帧、视频段落。

## License

本项目受版权保护，并采用自定义受限使用许可。

未经授权，不得再发布、镜像、二次打包、再授权、出售，或将本项目及其提示词、规则、工作流、文档、衍生内容用于商业产品、付费服务、SaaS、咨询交付、培训课程、市场上架或其他营利性用途。

如需再发布授权、商业授权、合作或其他许可，请联系：微信 `MARIOZHAOFAN`。

完整条款见 [LICENSE](LICENSE)。
