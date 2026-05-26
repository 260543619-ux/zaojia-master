# 造价大师 Zaojia Master

中国建设工程造价管理 Claude Code Skill，严格遵循 GB/T 50500-2024《建设工程工程量清单计价标准》。

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-orange)](https://claude.ai/code)
[![Standard](https://img.shields.io/badge/GB%2FT%2050500-2024-red)](https://ebook.chinabuilding.com.cn/zbooklib/book/detail/show?SiteID=1&bookID=155858)

## 简介

造价大师是一个面向中国建设工程造价管理全流程的 AI Agent Skill。严格遵循 GB/T 50500-2024 标准，覆盖从投资估算、设计概算、施工图预算、招标控制价、投标报价，到竣工结算、竣工决算、造价审计的全生命周期。同时兼容各省市现行定额体系和广联达等主流造价软件。

### 适用专业

- 房屋建筑工程
- 装饰装修工程
- 市政基础设施工程
- 通用安装工程（电气、给排水、暖通、消防）
- 园林绿化工程
- 城市轨道交通工程

### 核心能力

| 能力 | 说明 |
|------|------|
| 工程量清单编制 | 按GB/T 50500-2024编制四层级工程量清单（分部分项+措施+其他+税金） |
| 综合单价组价 | 人工+材料+机械+管理费+利润+风险，支持定额组价和市场询价 |
| 招标控制价/投标报价 | 最高投标限价编制，投标报价合理性审查 |
| 合同价款管理 | 单价合同/总价合同选择、风险分配、价款调整公式 |
| 变更签证处理 | 变更定价三优先原则、签证四要素、索赔费用计算 |
| 材料价差调整 | 造价信息法、市场询价法、综合系数法 |
| 竣工结算审核 | 结算文件编制与审核，常见审减点识别 |
| 造价审计 | 政府审计/第三方审计流程与要点 |
| 软件操作指导 | 广联达GTJ/GQI/GCCP、鲁班、斯维尔等 |
| 术语对照 | 200+核心中英造价术语 |

## 目录结构

```
zaojia-master/
├── SKILL.md                              # 核心入口文件（~220行+自检清单+输出模板）
├── README.md                             # 本文件
├── LICENSE                               # MIT 许可证
└── references/                           # 详细参考文件（按需加载，共11个）
    ├── gbt50500-2024.md                  # GB/T 50500-2024标准解读+新旧对比
    ├── 13-step-cost-management.md        # 估算→决算13步全流程
    ├── boq-and-pricing.md                # 工程量清单+综合单价详解（11省取费基数）
    ├── contract-and-risk.md              # 合同类型+风险分配+价款调整
    ├── change-order-and-claims.md        # 变更+签证+索赔
    ├── material-price-adjustment.md      # 材料价差调整方法
    ├── audit-and-settlement.md           # 结算+决算+审计
    ├── fee-composition.md                # 费用组成四级划分（8省取费对比）
    ├── software-and-quotas.md            # 广联达+鲁班+斯维尔+同望+纵横
    ├── zaojia-indicators.md              # 造价指标+含钢量+材料参考价+EPC/装配式
    └── glossary.md                       # 200+中英造价术语
```

## 安装方法

### 方式一：克隆到项目（推荐）

```bash
# 进入你的项目
cd your-project

# 确保存在 .claude/skills 目录
mkdir -p .claude/skills

# 克隆技能
git clone https://github.com/260543619-ux/zaojia-master.git .claude/skills/zaojia-master
```

### 方式二：全局安装（所有项目可用）

```bash
# 安装到用户级 skills 目录
mkdir -p ~/.claude/skills
git clone https://github.com/260543619-ux/zaojia-master.git ~/.claude/skills/zaojia-master
```

安装后，Claude Code 会在启动时自动发现该 Skill。下次对话中提到"帮我编一个工程量清单"、"审核这份结算"、"这个综合单价合理吗"等时，Skill 会自动激活。

### 方式三：通过 agentskill.sh 安装（如已发布）

```bash
/learn @260543619-ux/zaojia-master
```

## 触发方式

直接使用中文描述你的造价需求，Skill 会自动触发。例如：

- "帮我编制一个6层住宅楼的土建工程量清单"
- "审核一下这份投标报价的综合单价"
- "钢筋涨了20%，怎么调价差"
- "这个变更签证的费用怎么算"
- "GB/T 50500-2024和2013版有什么区别"
- "广联达GCCP怎么套定额"
- "竣工结算审核要注意什么"
- "暂列金额和暂估价有什么区别"

## 适用标准与依据

- **GB/T 50500-2024**《建设工程工程量清单计价标准》（2025年9月1日实施，替代GB 50500-2013）
- 各省建设工程计价定额（北京、上海、广东、深圳、浙江、江苏、四川、重庆等）
- 各省级工程造价信息（人工费指导价、材料信息价、机械台班参考价）
- GF-2017-0201《建设工程施工合同（示范文本）》

## 级别说明

| 文件 | 加载时机 | 大小 |
|------|---------|------|
| SKILL.md 正文 | Skill 激活时 | ~200行 |
| references/*.md | 按需加载（仅读相关文件） | 10个文件，合计~2000行 |

## 作者

- **维护者**: 260543619-ux
- **GitHub**: [@260543619-ux](https://github.com/260543619-ux)

## 许可证

MIT License — 详见 [LICENSE](LICENSE) 文件

## 贡献

欢迎提交 Issue 和 Pull Request：

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 创建 Pull Request

### 贡献方向

- 补充更多省份的定额体系说明
- 增加专业分包（幕墙、钢结构等）造价指导
- 完善 GB/T 50500-2024 的实务案例解读
- 增加典型造价指标数据（住宅/商业/工业/学校/医院等）
- 补充国际工程（FIDIC）造价对比分析（后续可选模块）
