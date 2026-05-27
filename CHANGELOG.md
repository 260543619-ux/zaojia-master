# Changelog

## v0.4.0

- 新增 `references/dwg-cad-handling.md`：DWG/CAD 三级处理能力模型（L1文本/L2几何/L3模型算量），能力判断流程，QCAD轻量提取操作指南，诚实表述模板
- 新增 `references/market-pricing-fallback.md`：信息价缺失时的五级定价降级方法，地区系数参考，主要材料暂估方法，定价输出格式规范
- `workflows/second-pass-review.md`：步骤4从纯定性扩展为定性+定量检查（合价验算、含量反算、单方验算、单价分布），增加基准匹配偏差判断逻辑
- `workflows/quantity-indicator-check.md`：新增步骤0 — 按清单文件规模选择处理策略（≤3/4-15/>15张计价表），按专业分组+重复编码跨sheet核查
- `input-specs/excel-boq.md`：新增广联达/GCCP标准导出格式快速识别（6类工作表+标准列结构），大型BOQ三级处理策略

## v0.3.0

- 新增 `references/quota-application-and-conversion.md`：定额套用与换算实操，清单量与定额量区分，端到端组价示例
- 新增 `references/rebar-flat-method-basics.md`：钢筋平法基础知识，含钢量异常信号及复核提示
- 新增 `references/standard-transition-check.md`：GB 50500-2013与GB/T 50500-2024过渡期混用风险检查
- 新增 `workflows/progress-payment-audit.md`：进度款审核Agent工作流，预付款扣回+质保金+甲供材
- 新增 `workflows/quantity-indicator-check.md`：算量指标反查Agent工作流
- 新增 `templates/progress-payment-audit-table.md`、`templates/quantity-indicator-check-table.md`
- 新增安装/市政清单专项检查规则

## v0.2.0

- 将 Skill 定位从知识参考扩展为文件审核工作流辅助
- 新增 `input-specs/`：定义清单、合同、结算书、签证变更、材料价格表的输入规范
- 新增 `workflows/`：提供结算审核、清单漏项检查、合同风险识别、签证审核、材料价格分析工作流
- 新增 `templates/`：提供结算审核报告、审减台账、合同风险台账、材料调差表等交付模板
- 新增 `examples/` 和 `test-cases/`：真实任务示例和人工验收用例
- 新增 `references/boq-check-rules.md` 和 `references/cost-management.md`
- 强化证据引用、缺资料处理、价格/定额防胡编和人工确认规则
- 新增二次审核工作流和质检报告模板

## v0.1.0

- 初始造价知识库版本
- 覆盖 GB/T 50500-2024、清单计价、综合单价、合同风险、变更签证、材料调差、结算审核等基础 reference
