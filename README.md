# After Sales Log Analyzer

**版本：** v7.8  
**作者：** Taro Wang (道工 DaoGong)  
**用途：** DIDA 售后运营团队专用的订单日志判读工具

## 功能描述

本 Skill 用于分析酒店预订订单的操作日志，生成标准化的 AP/AR Proposal 分析报告，帮助售后运营团队快速判断责任方并计算赔付金额。

## 核心能力

- ✅ 自动识别渠道责任/供应商责任/DIDA 责任
- ✅ 提取关键赔付金额和结算信息
- ✅ 生成标准化分析报告（支持 AP/AR Proposal 决策）
- ✅ 支持双订单号匹配（渠道订单号 + 供应商订单号）
- ✅ 智能识别人工沟通记录（排除系统日志）

## 使用方式

### 在 OpenClaw 中调用

```
请基于 URL 中的内容执行 after-sales-log-analyzer skill：http://tiger-api.didaadmin.com/api/v2/BookingManage/GetBookingOperationLogByToken?token=[TOKEN]
```

### 输出示例

```markdown
## 订单日志分析报告

**订单号：** 14334195083 / 1941164368217645056

⭐ **Conclusion** 到店前无房（酒店超售），供应商应赔付 57 USD，渠道争议金额从 1,935.92 调整为 609 最终归零

| 字段 | 内容 |
|------|------|
| **DisputeResponsibility** | Supplier |
| **SupplierCompensatePrice** | 57 USD |
| **AP Proposal** | -57 USD |
| **AR Proposal** | 0 |
```

## 字段说明

| 字段 | 说明 |
|------|------|
| DisputeContent | 争议内容（原订单配置 vs 实际执行） |
| DisputeResponsibility | 责任方（Channel/Supplier/DIDA） |
| SupplierCompensatePrice | 供应商应赔付金额 |
| CompensateChannelPrice | 应赔付渠道金额 |
| ChannelFeedback | 渠道沟通反馈 |
| SupplierFeedback | 供应商沟通反馈 |
| HotelFeedback | 酒店沟通反馈 |
| AP Proposal | 应付账款建议（Accounts Payable） |
| AR Proposal | 应收账款建议（Accounts Receivable） |

## 版本历史

- **v7.8** (2026-08-27): 全面扩充 Feedback 关键词清单
  - ChannelFeedback：新增邮件渠道/渠道邮件/电话渠道/渠道电话/渠道告知/表示/声称/通知，英文关键词
  - SupplierFeedback：新增供应商邮件/供应商电话/供应商表示/声称/通知/说明/解释，英文关键词
  - HotelFeedback：新增酒店邮件/酒店电话/酒店表示/声称/通知/说明，英文关键词
- **v7.7** (2026-08-27): 扩展 SupplierFeedback 识别规则
  - 新增关键词：`邮件供应商 `、`电联供应商`、`联系供应商`、` 供应商：`、`Suppliers:`、`Dear Supplier`
  - 处理 HTML 包裹：提取 `[服务轨迹 - 邮件]` 后的实际沟通内容
  - 排除内部操作日志（连玉婷/曹诗涵等修改金额的系统操作）
- **v7.6** (2026-08-27): 新增 ChannelFeedback 字段，明确 Feedback 字段识别规则
- **v7.5** (2026-08-27): 修复供应商日志遗漏问题
  - 处理 web_fetch security wrapper 包裹的 JSON
  - 同时匹配渠道订单号和供应商订单号（两者可能不同）
  - 深入挖掘所有日志，不遗漏任何供应商反馈记录
- **v7.4** (2026-08-27): 优化输出格式，移除 To ReOP 行
- **v7.3** (2026-08-27): 新增🌙To ReOP 操作建议行
- **v7.2** (2026-08-27): 新增币种缺失提示
- **v7.1** (2026-08-27): 简化输出格式，移除元数据字段
- **v7.0** (2026-08-26): 添加身份定义（DIDA 售后运营专家）
- **v1.0-v6.x** (2026-08-25~26): 迭代优化输出模板和识别规则

## 相关文档

- [Feedback 关键词清单](https://www.feishu.cn/docx/Ae2udj6AtoRX3FxkDRZcZMjNnwg)
- [OpenClaw 文档](https://docs.openclaw.ai)
- [OpenClaw Skills](https://github.com/openclaw/openclaw/tree/main/skills)

## 许可证

MIT License

## 联系方式

**作者：** Taro Wang (道工 DaoGong)  
**适用场景：** 酒店预订行业售后运营  
**专业领域：** 酒店行业 (Hospitality Industry)、旅游科技 (Travel Tech / Hotel Tech)
