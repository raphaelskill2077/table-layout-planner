# Table Layout Planner｜摆桌子布局规划

一个面向办公室、餐厅和培训室的 Agent Skill。它不会一上来就画图，而是先确认场景、带尺寸的平面图和唯一最高优先级，再按照真实家具包络、通道、动线与使用行为生成可核对的布局方案。

An Agent Skill for practical table and seating layouts in offices, restaurants, and training rooms.

## 能做什么

- 办公室：优化固定/共享工位、部门邻接、访客路线和高峰承载。
- 餐厅：结合 Party Mix、桌型、服务路线、容量与 RevPASH 评估布局。
- 培训室：在浅弧/U 形/岛式之间选择适合讲授、讨论或 Workshop 的方案。
- 使用真实桌椅尺寸、椅子活动空间和净通道，而不是只按桌面面积计算。
- 先提交文字方案供确认，再生成桌椅数量一致的平面图。
- 区分规划级、采购级和施工级精度，避免把概念方案误称为合规施工图。

## 首轮需要的信息

Skill 会优先确认三项：

1. 场景：办公室、餐厅或培训室。
2. 带尺寸的平面图：至少有一个可校准的真实尺寸，并标出门、柱、固定区域和不可移动设施。
3. 唯一最高优先级 P1：例如办公室空间利用率、餐厅营收、培训互动、高频讨论、容量或视线。

三项不齐时，它只会补充询问或提供测量指导，不会直接给出最终桌数和设计图。

## 工作流

```text
判断场景与 P1
    ↓
读取并校准平面图
    ↓
锁定固定结构与真实家具包络
    ↓
先布动线，再比较候选布局
    ↓
提交文字方案
    ↓
用户确认
    ↓
生成平面图并核对桌数、椅数和净距
```

## 文件结构

```text
table-layout-planner/
├── .gitignore
├── README.md
├── SKILL.md
└── references/
    ├── office.md
    ├── restaurant.md
    ├── restaurant-types.md
    ├── evaluation.md
    ├── evidence.md
    └── training-room.md
```

`SKILL.md` 是统一入口，只会按场景读取相应参考文件，避免同时加载三套规则。

## 安装

把整个 `table-layout-planner` 目录放入兼容 Agent Skills 的技能目录，并确保 `SKILL.md` 位于技能目录根部。

克隆仓库并进入仓库根目录后，可把 Skill 文件复制到 Codex 的用户级技能目录：

```bash
mkdir -p ~/.codex/skills/table-layout-planner
cp -R SKILL.md references ~/.codex/skills/table-layout-planner/
```

也可以在支持本地 Skills 路径的运行环境中直接指向本仓库目录。Skill 目录和 `SKILL.md` 的结构遵循 [OpenAI Skills 文档](https://developers.openai.com/api/docs/guides/tools-skills)。

## 使用示例

```text
使用 $table-layout-planner，帮我优化办公室工位。
最高优先级是提高高峰承载人数，我会上传带尺寸的平面图。
```

```text
使用 $table-layout-planner，重新规划这家火锅店的桌型。
最高优先级是提高可实现营收，不拆墙。
```

```text
使用 $table-layout-planner，设计 30 人高频讨论培训室。
我需要优先保证互动，并希望家具能快速切换布局。
```

## 设计边界

- 默认输出概念规划，不替代建筑、消防、疏散、无障碍、机电或餐饮卫生施工审查。
- 没有可靠尺寸时，不宣称精确容量、距离或合规。
- 不通过缩小家具、删除拉椅空间或压缩必要通道来增加座位。
- 外部研究和案例的适用边界见 `references/evidence.md`。

## 来源、商标与许可

- 仓库只链接和释义外部论文、标准及公开案例，不复制或再授权第三方原文。
- 文中品牌名称仅用于识别公开案例，不代表品牌背书、合作或官方标准。
- 当前发布包未替仓库所有者选择许可证。公开发布前可按期望的复用方式自行添加 `LICENSE`；第三方资料不随本仓库许可证重新授权。
