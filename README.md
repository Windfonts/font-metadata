# font-metadata

字体元数据中心，管理 Windfonts 字库中所有字体的结构化信息。

## 数据来源

元数据由 `fonts-packages` 仓库中的 `python-scripts/analyze-fonts.py` 通过 AI 分析生成，最终产物为 `font-mapping.json`，同步至 OSS（`wenfeng-fonts` bucket）。

## 目录结构

```
fonts/
  {NormalizedName}/
    meta.json          # 字体元数据
schemas/
  font-meta.schema.json
index.json             # 所有字体索引（normalizedName → 基本信息）
```

## meta.json 核心字段

| 字段 | 说明 |
|------|------|
| normalized_name | 唯一标识（如 `Almmsht`） |
| english_name / chinese_name | 英文名 / 中文名 |
| font_family | CSS font-family（`WF-{Name}` 格式） |
| foundry | 字体厂商（对应 font-foundries 仓库） |
| designer | 设计师 |
| release_year | 发布年份 |
| font_category | 字体分类（无衬线/衬线/手写体等） |
| font_tags | 风格标签（黑体/宋体/楷体等） |
| languages | 支持语言 |
| use_cases | 适用场景 |
| weights | 字重信息（含 font_weight 数值、版本文件） |

## 分类体系

- **font_category**：无衬线字体、衬线、粗衬线字体、脚本、Mono、手写体
- **font_tags**：黑体、宋体、楷体、隶书、拼音、硬笔手写、毛笔书法、卡通创意、其他
- **languages**：简体中文、繁体中文、英语、日文、韩文

## 关联仓库

- `fonts-packages` → 字体原始文件 + 构建工具（元数据由此生成）
- `font-licenses` → 授权信息（通过 normalized_name 关联）
- `font-foundries` → 厂商信息（通过 foundry 字段关联）
- `fonts-vault` → 平台数据库（通过 POST /api/sync 从 OSS 同步）
