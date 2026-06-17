# CUGB Doctoral Thesis Format

CUGB Doctoral Thesis Format is a Codex skill for prechecking and assisting Word-format doctoral theses against the China University of Geosciences (Beijing) doctoral thesis template.

It treats thesis formatting as a Word-structure problem, not just a font problem:

```text
DOC/DOCX thesis
-> section, margin, header, footer, and page-number audit
-> cover, abstract, TOC, heading, caption, formula, and reference checks
-> local Markdown/JSON precheck report
-> safer manual or agent-assisted revision
```

## Install

Clone this repository into your Codex skills directory using the skill name from `SKILL.md`.

Windows PowerShell:

```powershell
git clone https://github.com/keros68/cugb-doctoral-thesis-format-skill.git "$env:USERPROFILE\.codex\skills\cugb-doctoral-thesis-format"
```

macOS/Linux:

```bash
git clone https://github.com/keros68/cugb-doctoral-thesis-format-skill.git ~/.codex/skills/cugb-doctoral-thesis-format
```

Then start a new Codex thread and call:

```text
Use $cugb-doctoral-thesis-format to precheck this doctoral thesis DOCX.
```

## What It Produces

- Local precheck reports in Markdown and JSON.
- Section, page setup, margin, header/footer, page-number, and hidden-header audits.
- Cover, declaration, Chinese/English abstract, keyword, TOC, heading, caption, formula, punctuation, and GB/T 7714 reference checks.
- A structured issue summary that separates severe problems, errors, warnings, and manual-review items.
- Safer revision guidance for Word files without overwriting the original thesis.

## Quick Commands

Run the main local precheck:

```bash
python scripts/precheck_cugb_doctoral_thesis.py path/to/thesis.docx --output-dir output/precheck
```

Include a school HTML format report when available:

```bash
python scripts/precheck_cugb_doctoral_thesis.py path/to/thesis.docx --detection-report path/to/report.html
```

Run individual inspection helpers:

```bash
python scripts/extract_cugb_docx_format.py path/to/thesis.docx
python scripts/check_cugb_doctoral_docx.py path/to/thesis.docx
python scripts/summarize_cugb_detection_report.py path/to/report.html
```

Convert an old `.doc` file to `.docx` on Windows with Microsoft Word:

```powershell
powershell -ExecutionPolicy Bypass -File scripts/convert_doc_to_docx_with_word.ps1 -InputPath input.doc -OutputPath output.docx
```

## Template Scope

The bundled assets are based on the 2021 revised CUGB doctoral thesis writing guide and template. If the university, graduate school, or college publishes newer requirements, those newer files should override this repository.

## Asset Boundary

Code, skill instructions, scripts, and workflow notes written for this project are released under MIT License.

The files in `assets/` are school template and guide materials kept for format-checking calibration. Their original ownership and usage terms still apply; the MIT License for this repository does not relicense those template materials.

## Repository Layout

- `SKILL.md` - Codex skill instructions.
- `agents/openai.yaml` - UI metadata for compatible runtimes.
- `assets/` - CUGB template and writing-guide assets.
- `references/cugb-doctoral-format-rules.md` - extracted hard rules.
- `references/cugb-detection-failure-modes.md` - common detection-report failure modes.
- `references/cugb-revision-playbook.md` - suggested revision order.
- `scripts/precheck_cugb_doctoral_thesis.py` - main local precheck workflow.
- `scripts/check_cugb_doctoral_docx.py` - heuristic DOCX checker.
- `scripts/extract_cugb_docx_format.py` - Word structure and style extractor.
- `scripts/summarize_cugb_detection_report.py` - HTML detection-report summarizer.
- `scripts/convert_doc_to_docx_with_word.ps1` - Word COM conversion helper.

## Verify

```bash
python scripts/test_cugb_format_tools.py
```

## Limits

This skill does not guarantee school-system approval. Final checks should still use Microsoft Word field updates, exported PDF review, and the official school format-detection system.

Do not commit personal thesis files, student IDs, unpublished detection reports, or private documents to a public repository.

## License

MIT for project code and instructions. See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md).
