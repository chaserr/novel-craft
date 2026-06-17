# novel-craft

长篇小说写作与审稿 plugin。题材中性——校园青春、悬疑、武侠、科幻、历史、都市等长篇均可。每个具体小说项目通过项目级 `RTK.md` 注入题材气质、世界观、人物、风格基线。

📖 **完整使用指南**：[GUIDE.md](./GUIDE.md)（从 0 到 1 上手 + 工作流 + Skill/Agent 详解 + 常见陷阱 + FAQ）

## 心智模型

```
┌────────────────────────────────────────────────────┐
│              你的小说项目目录                       │
│                                                    │
│  RTK.md            ← 项目级规则（题材、风格、底线）  │
│  小说大纲.md       ← 全书骨架                      │
│  章节大纲.md       ← 逐章节点                      │
│  前情梳理.md       ← 已写章节的滚动摘要            │
│  伏笔清单.md       ← 已埋/已收的伏笔台账           │
│  经典语录.md       ← 留得下的句子                  │
│  人物档案/*.md     ← 角色的活体状态                │
│  写作技巧/*.md     ← 项目沉淀的写作技法            │
│  审稿报告/*.md     ← 历轮审稿沉淀                  │
│  <书名>-第X章.md   ← 章节正文                      │
└────────────────────────────────────────────────────┘
         ↑                            ↑
         │                            │
    ┌────┴────┐                  ┌────┴────┐
    │ skills  │                  │ agents  │
    └─────────┘                  └─────────┘
   写作主流程闭环              单一职责的审稿/写作角色
```

## 包含的 skill

| Skill | 触发场景 | 职责 |
|-------|---------|------|
| **novel-init**       | 新建小说项目时           | 初始化项目目录、生成 RTK.md 等模板 |
| **novel-write**      | 写第 X 章 / 续写 / 扩写  | 完整章节写作主流程：读资料 → 生成初稿 → 去 AI 味 → 输出定稿 |
| **zh-novel-polish**  | 章节定稿前最后一道       | 中文小说去 AI 味润色（八大叙事病灶 + 通用清单），novel-write 内部调用 |
| **novel-sync**       | 章节写完后               | 同步更新前情梳理、人物档案、伏笔清单、经典语录 |
| **novel-review**     | 审稿 / 找问题            | 分配并协调 novel-* agent 完成多维度审查 |

## 包含的 agent

12 个单一职责的写作/审稿角色。所有 agent 会在开始工作前优先读取项目 `RTK.md` 注入题材气质。

| Agent | 类比传统编辑岗 | 用途 |
|-------|--------------|------|
| **novel-architect**     | 总编           | 世界观、大纲、设定集、全书结构 |
| **novel-writer**        | 执笔者         | 根据大纲/设定生成正文 |
| **novel-memory**        | 连续性编辑     | 跨章/跨对话的状态档案 |
| **novel-auditor**       | 校对/审计员    | 逻辑、设定、时间线一致性 |
| **novel-pacer**         | 策划编辑       | 节奏曲线、爽点密度、章尾拉力 |
| **novel-plotter**       | 剧情编辑       | 伏笔追踪、支线管理、情感账单 |
| **novel-polisher**      | 文字编辑       | 风格统一、陈词滥调、句式打磨 |
| **novel-dialogist**     | 对话编辑       | 角色声音差异化、对白自然度 |
| **novel-scene-refiner** | 场景编辑       | 场景描写、感官层次、氛围 |
| **novel-reader**        | 试读读者       | 模拟目标读者的主观反馈 |
| **novel-researcher**    | 资料编辑       | 背景真实性、专业领域、时代细节 |
| **novel-curator**       | 质量管理       | 多轮审查的模式归纳、作者写作档案 |

## 内置写作技法 reference

plugin 自带 8 篇通用写作技法 reference（题材中性，存放在 `skills/novel-write/reference/`），`novel-write` 写章前会**按需查阅**：

| # | 文档 | 处理什么问题 |
|---|------|------------|
| 01 | 运镜 | 镜头顺序、远近切换、画面流向 |
| 02 | 感官 | 视/听/嗅/味/触五感的调用 |
| 03 | 细节 | 用动作、局部、习惯性反应立人物 |
| 04 | 心理 | 不靠独白写心理（借环境/动作/节奏） |
| 05 | 场景四字诀 | 细、顺、悬、张——场景描写的发力顺序 |
| 06 | 信息密度 | 删形容词、动作代状态、一句一信息 |
| 07 | 章末结构 | 6 种章末钩子结构，避免感悟收束模板 |
| 08 | 画面感总则 | 写一章前的 5 问 + 自查清单 |

**项目专属技法**（如校园的"楼梯转角符号"、武侠的"江湖切口"）由项目自己在 `写作技巧/` 目录下沉淀；写作时**项目专属优先级 > 通用 reference**。

## 安装

### 方式 A：从 GitHub 安装（推荐）

```bash
git clone https://github.com/chaserr/novel-craft.git ~/.claude/plugins/marketplaces/novel-craft
```

然后在 Claude Code 里执行 `/plugin install novel-craft@novel-craft`。

### 方式 B：本地路径启用

把仓库 clone 到任意位置（下文用 `<NOVEL_CRAFT_PATH>` 代指），在小说项目的 `.claude/settings.json` 里：

```json
{
  "extraKnownMarketplaces": {
    "novel-craft": {
      "type": "local",
      "path": "<NOVEL_CRAFT_PATH>"
    }
  }
}
```

### 方式 C：把 agent 软链到全局

```bash
ln -sf <NOVEL_CRAFT_PATH>/agents/novel-*.md ~/.claude/agents/
```

## 用法

```
# 新项目
> 帮我用 novel-craft 初始化一个新小说：题材<...>，目标读者<...>，主角<...>
(触发 novel-init)

# 写新章节
> 写第 X 章
(触发 novel-write)

# 章末同步
> 同步前情/伏笔/语录/档案
(触发 novel-sync)

# 审稿
> 审一下第 X-Y 章
(触发 novel-review，按需调用多个 novel-* agent)
```

## 设计原则

1. **题材中性**：不在 plugin 层内置任何具体题材（校园、玄幻、悬疑等）。题材气质从项目级 `RTK.md` 注入。
2. **资料 = 上下文**：所有写作/审稿都先读项目根目录的核心资料（RTK / 大纲 / 前情 / 伏笔 / 语录 / 人物档案 / 写作技巧）。
3. **写作即同步**：写完一章必须同步更新四类资料文件，否则下一章会基于过时上下文。`novel-sync` 把这个动作固化。
4. **单一职责 agent**：每个 agent 只做一件事，输出结构化报告。复杂任务由 skill 编排多 agent 协同。
5. **不替代作者判断**：所有 agent 只提建议、不擅自重写；冲突建议会同时呈现，让作者裁决。

## 支持一下

如果这个 plugin 帮到了你的写作，欢迎请我喝一杯咖啡，让 novel-craft 继续迭代下去 ☕

<table>
  <tr>
    <td align="center">
      <img src="./assets/donate-alipay.jpg" alt="支付宝" width="240"><br/>
      <sub><b>支付宝</b></sub>
    </td>
    <td align="center">
      <img src="./assets/donate-wechat.jpg" alt="微信支付" width="240"><br/>
      <sub><b>微信支付</b></sub>
    </td>
  </tr>
</table>

### 加入交流群

写长篇的路上不孤单，欢迎加入「AI 网文沟通群」一起讨论 AI 辅助写作、互相交流写作经验。

<table>
  <tr>
    <td align="center">
      <img src="./assets/wechat-group.png" alt="微信群" width="240"><br/>
      <sub><b>微信群（二维码会定期更新，如失效请提 issue 联系）</b></sub>
    </td>
  </tr>
</table>
