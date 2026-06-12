# CUGB Doctoral Thesis Format Skill

一个用于目标博士论文模板 Word 格式预检和排版辅助的 AI skill 或 agent 工作流。

它的目标不是替代学校格式检测系统，而是在提交前先做一轮本地体检：页眉页脚、分节、页边距、目录域、图表题、关键词、参考文献和常见标点问题，能在本地发现的尽量先发现。

## 适用场景

- 按内置博士论文模板检查 `.docx`。
- 提交学校检测前，先生成本地预检报告。
- 排查 Word 暗层问题：section、奇偶页页眉、页脚距、隐藏页眉残留。
- 把这个项目作为其他学校论文格式 skill 的起点；不绑定某一个 AI 工具。

## 模板版本

本仓库内置资产来自 `2021年修订` 博士学位论文书写指南和模板。后续如果学校或学院发布新版模板、归档要求或检测规则，应以最新通知为准，并更新 `assets/`、`references/` 和检查脚本。

## 许可与资产边界

本项目中由 keros68 编写的脚本、skill 说明和派生工作流笔记按 MIT License 发布；转载、fork、改名分发或二次打包时必须保留 `LICENSE`、`NOTICE.md` 和原始仓库链接。

`assets/` 下的学校模板和书写指南仅作为格式检查与规则校准材料保留，仍受其原始权属和使用条款约束。仓库的 MIT License 不额外授予这些第三方或学校模板材料的再授权权利。改造成其他学校版本时，请替换为你有权使用的模板与规范文件，并保留资产说明。

## 使用方式

这个项目的核心是 `SKILL.md + assets/ + references/ + scripts/`，可以给不同 AI agent 使用。

如果使用 Codex，可以把仓库克隆到 skills 目录：

```powershell
git clone https://github.com/keros68/cugb-doctoral-thesis-format-skill.git "$env:USERPROFILE\.codex\skills\cugb-doctoral-thesis-format"
```

也可以直接复制本仓库文件夹到：

```text
C:\Users\<你的用户名>\.codex\skills\cugb-doctoral-thesis-format
```

在支持 skill 的工具里，可以这样调用：

```text
使用 $cugb-doctoral-thesis-format 帮我预检这篇博士论文 DOCX。
```

指定 skill 后，不需要在提示里重复学校全称；规则已经在 skill 内部。

如果使用其他 AI 工具，可以把 `SKILL.md` 作为项目说明加载，把 `assets/`、`references/` 和 `scripts/` 一起提供给 agent。

## 一键预检

推荐优先运行：

```bash
python scripts/precheck_cugb_doctoral_thesis.py path/to/thesis.docx --output-dir output/precheck
```

输出两份文件：

- Markdown：给人看的本地预检报告。
- JSON：给后续脚本或 agent 继续处理。

如果已经有学校 HTML 检测报告，可以一起汇总：

```bash
python scripts/precheck_cugb_doctoral_thesis.py path/to/thesis.docx --detection-report path/to/report.html
```

## 预检和修改边界

当前脚本主要负责检查、汇总和定位问题。真正修改论文时，建议让 AI agent 先备份，再只改确定安全的项目，并输出改动清单。

适合自动或半自动处理：

- 页边距、页眉页脚文本、常规段落样式。
- 标题、图题、表题、参考文献的段落格式。
- 明显空段、部分全角/半角标点、目录域和 TOC 字段风险提示。

不适合盲目一键改：

- 封面个人信息、目录最终页码、交叉引用、公式编号。
- 图片位置、图题是否跨页、页末大空白、横向页是否被接受。
- 学校检测系统的隐藏规则，以及 Word/WPS/PDF 渲染差异。

所以它更适合做“先预检、再备份修复、最后人工复核”，不要把它包装成一键通过工具。

## 其他脚本

```bash
python scripts/check_cugb_doctoral_docx.py path/to/thesis.docx
python scripts/extract_cugb_docx_format.py path/to/thesis.docx
python scripts/summarize_cugb_detection_report.py path/to/report.html
```

旧版 `.doc` 转 `.docx`：

```powershell
powershell -ExecutionPolicy Bypass -File scripts/convert_doc_to_docx_with_word.ps1 -InputPath input.doc -OutputPath output.docx
```

## 改成别的学校

核心思路很简单：不要让 AI 凭空猜格式，给它足够的证据。

至少准备：

- 学校官方论文模板，最好有 `.doc/.docx`。
- 学校论文书写指南、格式规范 PDF 或网页。
- 一份格式检测报告，哪怕是不合格报告。
- 如果能找到，准备一篇已经通过格式审查的论文。

然后让 AI 做四件事：

1. 从模板和规范里抽取硬规则。
2. 从检测报告里提炼高频扣分点。
3. 把 `SKILL.md`、`references/`、`assets/` 替换成目标学校版本。
4. 改造检查脚本，先覆盖最容易被学校系统抓到的项目。

0 到 1 的问题是“skill 应该长什么样”。这个项目给出一个基准。后面 1 到 100，就是按每个学校的模板和报告继续模仿、校准、测试。

## 参考与致谢

本项目的组织思路参考了 [CTctikki/csu-thesis-format-Skill](https://github.com/CTctikki/csu-thesis-format-Skill)：把学校论文模板、格式规则、常见问题和检查脚本整理成一个可交给 AI agent 使用的工作流。

本仓库面向另一套博士论文模板重新整理规则、资产和检查逻辑，不包含原项目的学校模板或个人论文材料。

## 边界

- 本地预检不能替代学校最终检测。
- 页码、目录页码、页末大空白、图题是否和图片分离，仍然需要 Word 更新字段后导出 PDF 人工看。
- 不要把个人论文、学号、未公开检测报告提交到公开仓库。
