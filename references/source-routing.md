# 来源接入与两个现有技能的适配

## 执行前检查

检查当前工具目录，发现实际可调用的 WoS/Scholar 工具或工具搜索能力；不要调用仅存在于说明中的名字。可用来源路径按以下条件选择：

| 来源 | 可执行路径 | 条件与记录 |
|---|---|---|
| WoS | 实际存在且可验证结果来源的专用工具；用户已登录的浏览器；已配置并有权限的 Clarivate API | 专用工具返回结构化记录仍需验证数据库来源。浏览器操作先读其控制技能；API 先读当前官方端点说明。检查权限但不输出密钥。 |
| Google 学术 | 实际存在的 Scholar 工具；可访问 Scholar 的浏览器 | 工具后端若实际代理到其他库，不标成 Scholar。记录查询与结果页/条目链接。 |
| 补充数据库 | paper-lookup 对应来源 API；nature-academic-search 的实际可用 MCP | 用来补充发现、摘要、全文、DOI、引用网络；保留真实来源名。 |

WoS 浏览器机构登录与 WoS API 权限不是一回事；不因能登录网页就推定有 API 密钥。此技能不自动安装 MCP、修改浏览器配置或配置订阅。遇到登录/验证码，交由用户完成，不绕过；同一访问失败不反复重试。继续可独立执行的工作。

若用户允许 WoS **或者** Scholar，WoS 不可用但 Scholar 可用时直接继续 Scholar。若都不可用且未限定只用它们，说明缺口，用公开数据库做补充检索，清楚标注并交付指定库检索式；若明确“只限 WoS/Scholar”，正式结果必须有指定库记录凭据。此时补充结果只能作为单列的未确认线索，不声称满足指定来源要求。

`web.run` 普通网络搜索、`site:scholar.google.com` 搜索或生成的 Scholar 查询 URL **不等于已经运行 Google 学术检索**。原技能的 `search_webofscience` / `search_google_scholar` 是可能的工具名而非存在保证。官方元数据核验也不能补造 WoS 收录状态。

## 适配实现边界

- nature-academic-search：从 manifest 定位 multi-source-search；只有用户需要核验/导出时加载额外工作流。复用检索、去重、格式转换；用户明确来源优先，不强迫先走其他库。其来源分层是工作流默认，不是文章学术质量评级。
- paper-lookup：跨领域发现首选其 OpenAlex/Semantic Scholar 路由；Crossref 用于 DOI 和出版元数据。它没有 WoS/Scholar 原生后端，不能被包装成这两者。全文不足时再按需用 PMC、CORE、Unpaywall 路由。API 可用性和凭据要求以执行时文档及真实响应为准。
- 若需直接 API 实现，使用官方当前接口，检查状态码、分页、配额与响应字段；不复制未经核实的网页内部 API 作为永久接口。
- 去重优先 DOI（规范化大小写和 doi.org 前缀），无 DOI 时比较题名、作者、年份；不要把预印本与正式版本当作两项独立证据。

## 可改用的检索式示例

示例论点：“缺氧条件可能促进湖泊沉积物磷释放”。这些是查询模板，不是已执行结果。

WoS 高级检索：

```text
TS=((lake* OR lacustrine) AND sediment* AND (phosphorus OR phosphate) AND (anoxi* OR hypoxi* OR redox))
```

宽检索后分别增加 release/mobilization 或 retention/immobilization 等结局；避免只保留预设方向。通过当前界面选择日期范围和排序；需要 AB= 或 NEAR/n 时先确认当前数据库及接口支持，不把核心合集字段语法套到所有 API。

Google 学术分次查询：

```text
"lake sediment" phosphorus anoxia release
"lake sediment" phosphorus anoxia retention
"sediment phosphorus" redox review
```

最新轨增加近三年时间筛选，并在年份粒度筛选后核实边界日期。权威轨以相关性和“被引用”追踪为线索，再读原文判断。全文匹配不是覆盖全部全文的保证。

## 记录来源真实性

每次保存：query、source、执行日期、筛选/排序、数据库合集（适用时）、可核实的结果页或响应、实际检查条数，以及状态 success/no_results/unavailable/not_attempted。不保存认证令牌或会话秘密。只在确有记录时报告总命中数。

WoS 单篇核实尽可能保留 WOS accession/UT 和记录 URL，并注明来自检索、精确 DOI 核查还是用户导入记录。用户导出的 WoS 文件可作为来源证据，但不能称为本次在线检索。仅期刊被收录不证明该篇或该年份被收录。Google 学术收录同样不能推断 WoS 收录。

## 官方参考（编写时核对：2026-09-14）

- [WoS 核心合集检索字段](https://webofscience.help.clarivate.com/en-us/Content/wos-core-collection/woscc-search-fields.htm)：Topic 包含题名、摘要、作者关键词、Keywords Plus。
- [WoS 高级检索字段](https://webofscience.zendesk.com/hc/en-us/articles/26916347018257-Web-of-Science-Core-Collection-Advanced-Search-Field-Tags)
- [Clarivate WoS Starter API](https://developer.clarivate.com/apis/wos-starter)：需要 API key；执行时核实方案权限和字段。
- [Google Scholar 搜索帮助](https://scholar.google.com/intl/en/scholar/help.html)：日期筛选、引用追踪、相关论文与全文链接。
