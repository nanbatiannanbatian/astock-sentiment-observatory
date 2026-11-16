# A股情绪观测 · 渠道 / 网站 / 开源项目 / 可操作闭环

> 一份面向交易参考的 A 股市场情绪观测框架：把「散户情绪怎么看出来」「现成渠道有哪些」「开源项目哪个好」整理成一个可照着用的闭环，并对每一条数据做了一手核实。

[![Live Site](https://img.shields.io/badge/在线预览-GitHub%20Pages-2f6fbf?style=flat-square)](https://nanbatiannanbatian.github.io/astock-sentiment-observatory/)
[![Repo](https://img.shields.io/badge/源码-GitHub-181717?style=flat-square)](https://github.com/nanbatiannanbatian/astock-sentiment-observatory)
[![Version](https://img.shields.io/badge/版本-最终交付版_2026.11.16-1f9e5a?style=flat-square)](./A股情绪观测-最终交付版-20261116.html)
[![License](https://img.shields.io/badge/性质-研究框架_非投资建议-c98218?style=flat-square)](#-免责声明)

**在线阅读**：https://nanbatiannanbatian.github.io/astock-sentiment-observatory/

---

## 一、这个项目解决什么

A 股**没有官方情绪指数**。市面上所有「恐慌/贪婪」读数，都是用交易行为反推心理（涨跌家数、涨跌停、两融、期权隐波、股吧发帖……），各家自己加权成 0–100 分。所以本项目的立场是：

1. **不迷信单一指标**——跨站数字口径不同、不可直接比较，同一站点看历史分位才有意义。
2. **区分两种情绪**——短线情绪（涨停/炸板/连板）≠ 中长线情绪（两融/期权 IV/股债利差），混用会误判。
3. **从「工具清单」走到「可操作闭环」**——不仅列渠道和项目，还给出「四层怎么配合」「信号什么时候该扔」的判据。

报告经过**八轮交叉复核**（我方 × DeepSeek × 一手数据裁决），GitHub 项目全部用 API 核实存在性与活跃度，研报数字抓取转载原文，错误入口（已停更指数、404 项目、失效域名）逐一剔除或标注。

---

## 二、三行极简版（10 秒读懂）

- **看方向**：华泰温度计是否从恐慌区回升 —— 周度跟踪华泰策略周报。
- **看结构**：vibe-astock 连板高度 / 炸板率是否配合 —— 日度看板。
- **看人心**：oceanfury 股吧/雪球分歧度是否下降 —— 文本层，置信度最低。
- **防自欺**：任何组合信号先过 stockproof 的 point-in-time 检验（Spearman IC + 样本外稳定性）。

> 说明：student-index、bagholder50 只作反向观察，**不进入决策表**。

---

## 三、四层闭环

```
华泰阈值（判方向·周度） → vibe-astock（看结构·日度） → oceanfury（看人心·日频） → stockproof（防自欺·检验）
```

| 层次 | 工具 | 作用 | 周期 |
|---|---|---|---|
| 1 · 判方向 | 华泰情绪温度计 | 10% 恐慌线 + 回升「首次上穿」事件，判断是否底部区域 | 周度 |
| 2 · 看结构 | `simonlin1212/vibe-astock` | 连板高度、炸板率、晋级率，确认短线赚钱效应 | 日度 |
| 3 · 看人心 | `oceanfury/retail-sentiment-analysis` | 股吧/雪球文本情绪、分歧度、讨论热度 | 日频 |
| 4 · 防自欺 | `taoqizhang55-code/stockproof` | point-in-time 检验，防止情绪信号事后拟合 | 检验时 |

**方法论约束**：组合判断以周度为决策窗口；短线信号仅用于确认中长线方向，不单独作为中长线依据。表中所有结论都是**状态描述**，不是买卖或仓位指令。

**信号何时该扔**：看信号与未来 5/10/20 日收益的 Spearman IC 是否显著、Walk-Forward 样本外是否稳定；IC 不显著先按「5 日平滑 + 周频口径」重跑，仍不显著再放弃（参照申万金工 2026-10 研报标准，非本报告独立验证）。

---

## 四、渠道速查

| 想看的维度 | 去哪里看 |
|---|---|
| 中长线温度阈值 | 华泰策略周报（搜「华泰 A股情绪温度计」） |
| 恐贪辅助共振 | 韭圈儿 APP 搜「恐贪指数」；yyqyx.com / baifenwei.com |
| 短线投机热度 | 同花顺输入代码 **883404**（只看阴阳线与分位） |
| 散户文本情绪 | 东方财富股吧人气榜 / 雪球热股榜 |
| 杠杆资金意愿 | 交易所两融数据 / 龙虎榜 / 中国结算开户数 |
| 期权隐含波动率 | 通达信 V050Z / TZ001 / TZ004；期权论坛 QVIX（optbbs.com，非官方复算） |

> ⚠️ 已核实避坑：**iVIX（000188）2018-02 起停更勿用**；**000993 是全指信息技术指数，不是波指**；悟道罗盘原 URL 实测失效，只给产品名不写死链接。各入口可用性仅反映 2026-11-16 实测状态。

---

## 五、开源项目（均经 GitHub API 核实，未在本环境实测运行）

| 项目 | 用途 | 状态 |
|---|---|---|
| [simonlin1212/vibe-astock](https://github.com/simonlin1212/vibe-astock) | 短线看板（主推） | 1051★/283 fork，Apache-2.0，持续维护，唯一接近「实盘辅助」标准 |
| [oceanfury/retail-sentiment-analysis](https://github.com/oceanfury/retail-sentiment-analysis) | 股吧/雪球文本情绪 | 0★，文档完整但停更，需自配 Cookie，可参考架构不宜原样生产 |
| [quantskills/skill-student-index](https://github.com/quantskills/skill-student-index) | 擦鞋童反向观察 | 0★，零依赖可离线演示，有效性未经统计检验，不进决策表 |
| [toocoldcc/bagholder50](https://github.com/toocoldcc/bagholder50) | 拥挤度反向观测 | 32★/MIT，冷西西韭菜50指数，避雷工具非买入名单，不进决策表 |
| [taoqizhang55-code/stockproof](https://github.com/taoqizhang55-code/stockproof) | 可验证性骨架 | point-in-time 十年回测 + 近 2 年锁定，防过拟合 |
| [akfamily/AKShare](https://github.com/akfamily/akshare) 等 | 数据基座 / 自建骨架 | 另含 ashare-sentiment（5★）、baoma 散户指数（14★）、Market-Sentiment-Index（PCA 残差法） |

**已剔除**：`hyan1985/MarketMonitoring`（404）、`redfox-community/stock-feed`（404）；`marketmood` 统一指向其改名后的 `stockproof`。

---

## 六、目录结构

```
.
├── index.html                            # 站点入口（= 最终交付版副本，访问根路径即见报告）
├── A股情绪观测-最终交付版-20261116.html   # 唯一最终交付的报告原件
├── echarts.min.js                        # 本地 ECharts 5.6.0，图表完全离线自包含
├── README.md                             # 本文件
├── .gitignore                            # 排除 .workbuddy/ 等内部数据
└── archive/                              # 8 份迭代过程稿（初版 → 完整版 → 各轮加固），请勿引用
```

分支：`main`（基线）与 `release1.01`（发布分支），当前均指向同一提交。GitHub Pages 从 `release1.01` 根目录构建。

---

## 七、本地运行

纯静态站点，无后端、无数据库。任选一种：

```bash
# 方式一：Python 内置服务器（零依赖）
python -m http.server 8080 --bind 127.0.0.1
# 浏览器打开 http://127.0.0.1:8080/

# 方式二：直接双击 index.html 用浏览器打开（图表用本地 echarts.min.js，同样可用）
```

> 图表库 `echarts.min.js` 已本地化，不依赖任何境外 CDN，断网也能正常渲染 star 对比图。

---

## 八、技术说明

- **零外部运行时依赖**：报告内嵌 CSS 与图表配置，唯一 JS 依赖 ECharts 已下载到本地（`echarts.min.js`，约 1MB）。原先引用的 `cdn.jsdelivr.net` 在实测中不可达（HTTP 000），故做了本地化修复。
- **数据静态化**：报告中的 star 数、阈值、IC 参数、实证数字均为核实当时的静态快照（2026-11-16），不接实时接口；引用时请核对最新数据。
- **部署**：GitHub Pages，`release1.01` 分支根目录。

---

## 九、免责声明

以上内容基于公开数据和量化分析，仅供参考，**不构成投资建议**。市场有风险，投资需谨慎；任何投资决策应结合个人风险承受能力、资金状况和投资目标独立判断，必要时咨询持牌专业机构，过往表现不预示未来收益。

推荐的开源项目（尤其 0–1 star 项目）**未经生产环境验证，也未在本环境实测运行**，使用前请自行评估代码质量、数据合规与爬虫合规风险；研报阈值与回测数字为非一手转载，需核对 PDF 原文。组合信号判断表是解释框架，不是交易信号；表中措辞均为状态描述，非操作指令。
