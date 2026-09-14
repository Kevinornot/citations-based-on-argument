# Citations based on argument

按论点寻找领域权威与最新文献的 Codex / Agent Skill。支持摘要或正文证据，不限制 Nature、Science、Cell 或其他期刊系列。

## 功能

- 将段落拆成可检索子论点，分别检索权威研究和最新进展（默认近三年）。
- 优先使用实际可用的 Web of Science 或 Google Scholar 接入。
- 兼容 `nature-academic-search` 与 `paper-lookup`；通过现有能力补充发现、核验 DOI、定位摘要和全文。
- 区分直接支持、条件性支持、背景相关、相反证据与未核验候选。
- 输出文献链接、证据位置、入选理由、适用条件及检索来源记录。

## 安装

```bash
npx skills add https://github.com/Kevinornot/citations-based-on-argument --skill citations-based-on-argument
```

也可下载本仓库，将 `SKILL.md`、`agents/` 和 `references/` 放入 `~/.codex/skills/citations-based-on-argument/`。

## 使用

```text
使用 $citations-based-on-argument，为论点“缺氧促进湖泊沉积物磷释放”
检索领域权威和近三年的文献，优先 WoS 或 Google 学术，不限期刊。
摘要或正文相关均可，注明证据位置，并列出相反证据与适用条件。
```

## 接入条件与兼容

本仓库提供检索与证据判断流程，不附带 WoS 订阅、API 密钥或 Google Scholar 服务。WoS 需要有效机构登录或相应 API 权限；Google Scholar 需要可用的浏览器或工具接入。无法访问时会明确标注，不把其他数据库结果冒充 WoS/Scholar 命中。

两个配套技能不包含在本仓库中。若已安装，按其名称定位并按需复用；说明中的相对路径对应创建时的本地布局，其他安装布局可通过当前技能清单定位。缺少配套技能时，可使用实际可用的检索工具继续，来源限制仍然有效。

## 文件

- [SKILL.md](SKILL.md)：技能入口。
- [来源接入与兼容](references/source-routing.md)：数据库路由、检索式和来源记录。
- [证据核验与输出](references/evidence-output.md)：证据分类、记录字段和交付形式。
- [界面信息](agents/openai.yaml)：显示名称与默认调用示例。

## 验证范围

已完成技能格式校验、依赖路径检查及五种静态情境复核。尚未完成 WoS/Google Scholar 的端到端检索实测；实际结果取决于接入权限与工具可用性。
