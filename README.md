# CUGB Doctoral Thesis Format Skill

一个用于中国地质大学（北京）博士学位论文 Word 格式预检和排版辅助的 Codex skill。

它的目标不是替代学校格式检测系统，而是在提交前先做一轮本地体检：页眉页脚、分节、页边距、目录域、图表题、关键词、参考文献和常见标点问题，能在本地发现的尽量先发现。

## 适用场景

- 按《中国地质大学（北京）博士学位论文模板》检查 `.docx`。
- 提交学校检测前，先生成本地预检报告。
- 排查 Word 暗层问题：section、奇偶页页眉、页脚距、隐藏页眉残留。
- 把这个项目作为其他学校论文格式 skill 的起点。

## 安装

把仓库克隆到 Codex skills 目录：

```powershell
git clone https://github.com/keros68/cugb-doctoral-thesis-format-skill.git "$env:USERPROFILE\.codex\skills\cugb-doctoral-thesis-format"
```

也可以直接复制本仓库文件夹到：

```text
C:\Users\<你的用户名>\.codex\skills\cugb-doctoral-thesis-format
```

安装后可以这样调用：

```text
使用 $cugb-doctoral-thesis-format 帮我预检这篇中国地质大学（北京）博士论文 DOCX。
```

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

## 边界

- 本地预检不能替代学校最终检测。
- 页码、目录页码、页末大空白、图题是否和图片分离，仍然需要 Word 更新字段后导出 PDF 人工看。
- 不要把个人论文、学号、未公开检测报告提交到公开仓库。
