# DWG/CAD 图纸处理策略与能力边界

本文件用于处理施工图纸（DWG/DXF/PDF 格式）时的能力判断和工作策略。它不替代 AutoCAD、GTJ/GQI、BIM 软件或专业图纸阅读，而是帮助 Agent 诚实地判断"我能从图纸中得到什么"，避免过度承诺或直接放弃。

## 一、三级图纸处理能力

| 能力等级 | 条件 | 能做什么 | 不能做什么 |
|---|---|---|---|
| **L1: 文本提取** | 有 QCAD / LibreCAD / DWG TrueView 可读取 DWG | 提取图层名、块参照名称、文字实体内容、线/多段线数量和类型统计 | 不能测量长度/面积/距离，不能识别构件类型，不能理解图纸比例 |
| **L2: 几何提取** | 有可量测的 DXF 文件或有比例标注的 PDF 图纸 | L1 全部能力 + 读取线段端点坐标、多段线顶点、圆/弧参数，可据此计算线性尺寸和封闭区域面积 | 不能自动识别构件（柱/梁/板/墙），不能处理图纸重叠/遮挡，不能理解标高关系 |
| **L3: 模型算量** | 有 GTJ / GQI / GCCP / 鲁班 / 斯维尔导出的工程量明细表（Excel/CSV） | 逐构件复核清单工程量，按楼层/构件类型分组对比，定位量差来源 | 需要人工核对扣减规则、节点构造、软件设置参数 |

## 二、能力判断流程

接到"复核图纸工程量"任务时，按以下顺序判断：

```
1. 是否有 GTJ/GQI/GCCP 导出的工程量明细表（Excel/CSV）？
   ├── 是 → L3 模式：逐构件分组对比，标记偏差 >5% 的项
   └── 否 → 继续判断

2. 是否有 DWG/DXF 文件？
   ├── 已安装 AutoCAD → 优先 3A.2（AutoLISP 算量）或 3A.3（DXF + ezdxf）
   ├── 有 QCAD/LibreCAD（无 AutoCAD）→ L1 模式：安装工具，执行轻量实体提取
   ├── DWG 可转为 DXF（无 CAD 工具）→ L2 模式：提取几何数据做线性/面积验证
   └── 仅有 PDF 图纸 → 降级为 L1：提取 PDF 文本，标注"无法量测"

3. 仅有图片/扫描件图纸？
   └── 无法进行任何复核。输出"需 CAD 格式图纸或 GTJ 导出明细表"
```

## 三-A、AutoCAD 原生方案（用户已安装 AutoCAD 时优先使用）

造价人员大概率已安装 AutoCAD，以下方案无需额外购买或许可，优先于 L1 QCAD 路线。

### 3A.1 检测 AutoCAD 安装

| 平台 | 检测方式 |
|---|---|
| macOS | 检查 `/Applications/Autodesk/AutoCAD 2024/AutoCAD 2024.app`（年份按实际版本替换） |
| Windows | 注册表 `HKLM\SOFTWARE\Autodesk\AutoCAD` 或检查 `C:\Program Files\Autodesk\` |

检测到 AutoCAD 后，优先使用以下两种方案，跳过 QCAD 安装。

### 3A.2 轻量算量方案（AutoLISP，免费）

1. 下载 [ferhatatesmech/AutoCAD-Quantity-Takeoff](https://github.com/ferhatatesmech/AutoCAD-Quantity-Takeoff)（GitHub 开源）
2. AutoCAD 内 `APPLOAD` → 加载 `quantity.lsp`
3. 按图层选择对象 → 自动汇总长度/面积 → 导出 CSV
4. 将 CSV 与清单量做差值对比

适用场景：按图层分构件（如 S-COLUMN、S-BEAM）的 DWG，可快速提取各图层多段线面积和线段长度。

### 3A.3 DXF 导出方案（ezdxf 解析）

1. AutoCAD 打开 DWG → `SAVEAS` → 选择 DXF 2018 格式
2. Python `ezdxf` 读取 DXF → 按图层提取多段线面积/长度
3. 与清单量做差值对比

```python
# 示例：按图层汇总多段线面积
import ezdxf
doc = ezdxf.readfile("exported.dxf")
msp = doc.modelspace()
areas = {}
for e in msp.query("LWPOLYLINE"):
    if not e.closed:
        continue
    layer = e.dxf.layer
    areas[layer] = areas.get(layer, 0) + e.get_area()
```

DXF 方案的优势：全命令行化、可批量、无 GUI 依赖，适合 CI/脚本集成。

## 三、L1 轻量实体提取操作指南（无 AutoCAD 时的备选方案）

QCAD 试用版启动时有约 15 秒延迟，频繁调用时不可行，仅适合少量文件的单次提取。批量处理优先使用 3A.2 或 3A.3。

### 安装 QCAD（macOS）

```bash
brew install --cask qcad
```

### 执行实体提取

```bash
# 注意：dwg2csv 实际路径在 QCAD.app/Contents/Resources/ 下，非 scripts/Proc/Dwg2Csv/
qcad -no-gui -autostop Resources/scripts/Proc/Dwg2Csv/dwg2csv.js <file.dwg> -o <output_dir>
```

### 提取后分析

对提取的 CSV 数据进行以下分析：

1. **图层完整性检查**：是否包含建筑/结构/给排水/电气/暖通对应的图层命名（如 `柱`/`梁`/`板`/`墙`/`S-WALL`/`COLUMN`）
2. **关键词审计**：统计文字实体中"钢筋"、"混凝土"、"C30"、"C35"、"DN"等关键词出现次数，用于侧面验证清单项是否在图纸中有对应内容
3. **块参照统计**：统计 `INSERT` 类型实体的块名，用于核对设备/门窗/管件是否在图纸中存在
4. **实体数量量级校验**：总实体数 < 1000 可能图纸不完整或比例异常；> 50000 通常为完整施工图

### L1 输出格式

| 检查项 | 方法 | 输出 |
|---|---|---|
| 专业覆盖 | 核对图层名 vs 清单涉及专业 | 某专业（如消防水）图纸存在/缺失 |
| 关键词线索 | 文字实体关键词计数 | "钢筋" 681 次，"混凝土" 455 次 |
| 设备线索 | 块参照名称统计 | 电气系统块 2331 个，阀门块 668 个 |
| 实体量级 | 总实体数 | 338,893 个实体（17 张 DWG 合计） |

## 四、L3 GTJ 导出明细表对比流程

当用户提供了 GTJ/GQI 导出的工程量明细表时：

1. 按清单编码分组汇总 GTJ 算量结果
2. 与招标清单逐项对比，计算量差 = (GTJ量 - 清单量) / 清单量 × 100%
3. 标记：
   - 量差 > 15% 且清单量更大 → 疑似清单多算
   - 量差 < -15% 且 GTJ 量更大 → 疑似清单漏算
   - 量差在 ±5% 内 → 暂采用清单量
4. 不直接修改清单量，仅输出差异表和需人工复核项

## 五、诚实表述模板

### 当仅有 DWG 且只能做 L1 提取时

> "本次使用 QCAD 对 N 张 DWG 进行了轻量实体提取（图层/文字/块参照），合计 X 个实体、Y 条文字样本。这些信息可用于核对图纸专业范围、关键词线索和设备块统计，但**不能**换算成清单工程量。
>
> 复核工程量以招标清单量为准，CAD 提取结果用于标注风险等级和复核范围。如需严格意义上的'施工图重新算量'，还需要 GTJ/GQI 导出的工程量明细表或可量测的 DXF 图纸。"

### 当完全没有 CAD 可读文件时

> "当前仅有 PDF/图片格式图纸，无法进行任何工程量复核。建议：
> 1. 提供 GTJ/GQI/鲁班/斯维尔导出的工程量明细表（Excel 格式）
> 2. 或提供 DWG/DXF 格式施工图纸
> 3. 或确认以招标清单量为准，仅进行价格复核"
