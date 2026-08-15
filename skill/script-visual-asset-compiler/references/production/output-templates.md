# Output Templates

Use this file when the user asks for a full asset package, prompt-only output, industrial prompt documents, canvas blueprints, or generation manifests.

## Full Asset Package

```text
## 导演拆解
- 故事核心:
- 类型 / 时代 / 世界规则:
- 视觉基调:
- 角色关系与视觉对比:
- 场景逻辑:
- 道具逻辑:

## 分镜资产地图
| Beat | 剧情功能 | 角色 | 服装 | 生物/载具 | 场景/视角 | 关键道具 | 分镜/视频 | 连续性备注 |

## 连续性记忆图谱
- 角色:
- 服装:
- 生物/怪兽:
- 场景:
- 道具:
- 载具:
- 色卡/材质DNA:
- 空间/门槛/路径:
- 风格锁:

## Project Profile / 视觉DNA
- profile_id:
- world:
- materials:
- colors:
- lighting:
- style_constraints:
- qa_focus:

## 电影感风格层
- 风格来源:
- LOOK 锁定:
- 适用边界:
- 模型适配:

## 角色资产
### CHR-01 角色名
- 角色功能:
- 形象锁定:
- 服装状态:
- 连续性要点:
- 服装设计图提示词:
  - CST-01_DESIGN:

## 场景资产
### SCN-01 场景名
- 剧情功能:
- 空间母版:
- 场景连续性卡:
- 必需视角:
- 生图提示词:

## 关键道具资产
### PRP-01 道具名
- 剧情功能:
- 造型锁定:
- 材质 / 标记 / 尺寸:
- 连续性要点:
- 生图提示词:
  - 45°侧视图:
  - 背视图:

## 生物/怪兽资产
### CRE-01 名称
- 剧情功能:
- 物种/结构:
- 尺度:
- 标志性记忆点:
- 材质/表面:
- 生图提示词:

## 载具资产
### VEH-01 名称
- 剧情功能:
- 驾驶/运动逻辑:
- 轮廓与尺度:
- 机械/动力结构:
- 生图提示词:

## 色卡/材质DNA
### DNA-01 项目视觉DNA
- 色彩:
- 材质:
- 光线:
- 禁止漂移:
- 生图提示词或文本规范:

## 分镜帧资产（仅当用户需要带人物画面）
### SHOT-01_FRAME-01 帧名
- 剧情功能:
- 引用资产:
- 视觉中心:
- 电影感 LOOK:
- 连续性要点:
- 生图提示词:

## 工业化提示词文档（用户需要节点/文件时）
- Prompt Body:
- 参数建议:
- 实操注意:
- 跑歪兜底加强:
- 版本:

## Canvas Blueprint（仅当用户需要画布/节点/CLI）
- mode:
- canvas_tool:
- sections:
- node_index:
- execution_notes:

## 缺口与假设
- ...
```

## Prompt-Only Output

```text
## 生图提示词

### CHR-01 / CST-01 角色名 - 服装状态
- 服装设计图:

### SCN-01 场景名
- SCN-01_MASTER:
- SCN-01_VIEW-xx:

### PRP-01 道具名
- 45°侧视图:
- 背视图:

### CRE-01 生物/怪兽名
- 多视图设定:

### VEH-01 载具名
- 多视图设定:

### DNA-01 视觉DNA
- 色卡/材质/灯光规范:

## 假设
- ...
```

## Generation Manifest

```text
| Asset ID | 类型 | 名称 | 视图 | 文件名/结果 | 备注 |
|---|---|---|---|---|---|
| CST-01 | costume | 角色名-服装状态 | design | CST-01_design.png | |
| SCN-01 | scene | 场景名 | MASTER | SCN-01_MASTER.png | |
| PRP-01 | prop | 道具名 | 45deg | PRP-01_45deg.png | |
```
