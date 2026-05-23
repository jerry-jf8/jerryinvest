# 🐼 Jerry Fang 投研

> AI 驱动的美股每日简报系统，由 Claude + Cloudflare Workers + Yahoo Finance 驱动。

---

## 功能概览

| 功能 | 说明 |
|------|------|
| 📊 大盘指数 | 实时拉取主要指数（SPY / QQQ / DIA 等） |
| 😱 恐慌指数 | VIX 实时数据 + 情绪解读 |
| 🏭 板块涨跌 | 11 个 SPDR ETF 板块紧凑排行 |
| 📈 个股追踪 | 自选股卡片/列表双视图，MA5/MA20/MA60，30 日走势图 |
| ✨ AI 评价 | Claude Sonnet 生成个股评价（每人每日 4 次） |
| 🔮 大师会诊 | 5 位投资大师视角并行分析（每人每日 5 次） |
| 📬 定时推送 | 每个交易日 4 次推送 + 周报，支持 PushPlus + Resend |

---

## 技术栈

```
前端         Netlify 托管的静态 HTML/ Github Pages （无框架）
后端         Cloudflare Workers（无服务器）
数据源       Yahoo Finance API
AI 模型      Claude Sonnet（评价/会诊）· Claude Haiku（推送）
推送         PushPlus（微信）· Resend（邮件）
```

---

## 文件说明

```
index.html    前端页面，部署到 Netlify
worker.js     后端 Worker，部署到 Cloudflare
```

---

## 部署方法

### 1. 前端（Netlify / Github Pages）
把 `index.html` 拖拽上传到 Netlify，或连接本仓库手动触发部署。每月限制上传次数，每月上传不要超过12次。
或者直接在 Github 内发布Pages

### 2. 后端（Cloudflare Workers）
1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. Workers & Pages → Create → 粘贴 `worker.js` 内容
3. Settings → Variables 配置以下环境变量：

| 变量名 | 说明 |
|--------|------|
| `ANTHROPIC_API_KEY` | Anthropic API Key |
| `PUSHPLUS_TOKEN` | PushPlus Token |
| `PUSHPLUS_TOPIC` | PushPlus 群组 Topic（可选） |
| `RESEND_API_KEY` | Resend API Key |
| `EMAIL_FROM` | 发件人地址（需在 Resend 验证） |
| `EMAIL_RECIPIENTS` | 收件人地址，多个用逗号分隔 |
| `ACCESS_CODE_RAW` | 多次调用密码 |

### 3. 配置前端后端地址
打开页面 → 右上角 ⚙ 设置 → 填入你的 Worker 地址。

### 4. Cloudflare Cron 触发器
在 Worker → Triggers → Cron 添加以下规则（交易日）：

```
15 13 * * 1-5   盘前速递（美东 09:15）
30 14 * * 1-5   开盘简报（美东 10:30）
30 16 * * 1-5   盘中简报（美东 12:30）
30 18 * * 1-5   收盘简报（美东 14:30）
30 4  * * 5     周报（周六美东 7:00）
```

---

## 接口说明

```
GET  /api/health                          健康检查
GET  /api/batch?symbols=TSLA,AAPL,^VIX   批量拉取行情
POST /api/ai-generate                     AI 评价
POST /api/guru-analysis                   大师会诊
GET  /api/trigger-email?type=preopen      手动触发推送
GET  /api/preview-markdown?type=weekly    预览推送内容
```

---

## 免责声明

本项目数据来源 Yahoo Finance，AI 内容由 Claude 生成，**其回答未必准确无误。AI生成的内容推荐仅供参考，不构成任何专业的财务或投资建议。股票投资存在重大风险，用户须对自身的投资决策及由此产生的财务损失承担全部责任。本分享仅供学习和参考，不构成任何投资建议。**

---

© 2026 Jerry Fang · All rights reserved
