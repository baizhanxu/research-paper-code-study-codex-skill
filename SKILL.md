---
name: research-paper-code-study
description: Guide Codex through research learning sessions that combine paper reading, project-code archaeology, paper-to-code mapping, reproducibility planning, and concept-first technical explanation. Use when the user asks to read or explain a research paper together with its codebase, summarize a method's technical route, map paper modules to files/classes/functions/configs, understand or reproduce an ML/AI/robotics/scientific project, compare papers by implementation feasibility, or maintain durable research-study notes.
---

# Research Paper Code Study

## Overview

Use this skill to help a user learn a research work end to end. Reconstruct the method from primary sources, inspect the real project implementation, align paper concepts with code paths, and explain the work in a way the user can reuse for study, reproduction, or future research.

Default to the user's language. If the user writes in Chinese, answer in clear Simplified Chinese unless they request another language.

## Core Rules

1. Ground claims in source material.
   - Prefer the local paper, official project page, official repository, README, docs, configs, scripts, and tests over generic memory.
   - Browse current primary sources when the user asks for latest work, online papers, project pages, repository state, or source citations.
   - Separate `paper says`, `code implements`, and `inference / likely intent`.
2. Read the real runtime path before explaining code.
   - Start from `README`, `docs`, install files, CLI/demo scripts, config files, and tests.
   - Trace entrypoints to dataset loading, model construction, training/inference loops, loss/objective code, evaluation, export, and visualization.
   - Do not assume file names imply behavior; verify by reading call sites.
3. Teach before overwhelming.
   - Start with position and motivation: what problem this work solves and why the design exists.
   - Then layer pipeline, module details, formulas, implementation, and limitations.
   - Use toy examples or intuitive analogies before dense math when the user is learning a new concept.
4. Keep the task scoped.
   - If the user asks for one paper or one module, stay inside it unless comparison is necessary.
   - If the material is large, produce staged checkpoints and continue through the agreed roadmap.
5. Make reproduction actionable.
   - Identify environment, dependencies, data, checkpoints, commands, expected outputs, hardware assumptions, and likely failure points.
   - Only run commands when useful and allowed; report what was actually run and what remains unverified.

## Mode Router

Choose the smallest mode that satisfies the request.

| User intent | Mode | Output focus |
|---|---|---|
| "讲这篇论文", "技术路线", "paper walkthrough" | Paper Technical Route | Problem, input/output, pipeline, modules, training/inference, limitations |
| "看这个项目", "代码结构", "入口在哪" | Codebase Orientation | Repository map, entrypoints, runtime path, key files |
| "结合论文和代码讲", "论文模块对应代码" | Paper-Code Alignment | Module-to-file mapping, implementation gaps, verified call graph |
| "帮我复现", "跑 demo", "训练/评估" | Reproduction Plan or Execution | Commands, dependencies, data/checkpoints, outputs, troubleshooting |
| "比较几篇工作", "哪个更适合复现/改进" | Comparative Study | Shared axes, paper differences, implementation feasibility |
| "整理成笔记", "维护学习记录" | Durable Notes | Markdown notes, glossaries, weak points, follow-up tasks |

If the user provides only a broad direction and no concrete paper/repo, first propose a lightweight study plan and ask for or search primary sources as appropriate.

## Intake Checklist

Gather only what is needed for the current mode:

1. Source material:
   - Paper PDF, arXiv/OpenReview/official page, supplementary material.
   - Repository URL or local path.
   - Existing notes, slides, issue threads, checkpoints, datasets, or experiment logs.
2. Learning target:
   - Overview, deep technical route, code walkthrough, reproduction, comparison, or durable notes.
3. User level and output preference:
   - Beginner-friendly, formula-heavy, code-heavy, or research-summary style.
4. Constraints:
   - Time budget, hardware, unavailable data/checkpoints, offline-only, no execution, or no file writes.

Avoid blocking on perfect intake. Make reasonable assumptions, state them, and continue when the next step is low risk.

## Paper Reading Procedure

1. Identify bibliographic and artifact handles:
   - Title, authors, venue/date if available, paper URL, code URL, project page, supplement.
2. Extract the method skeleton:
   - Research problem and gap.
   - Input assumptions and output representation.
   - High-level pipeline.
   - Key modules and their responsibilities.
   - Training data, supervision, losses/objectives, evaluation metrics.
   - Inference-time data flow.
3. Preserve exact technical names:
   - Module names, model names, loss names, datasets, config names, algorithms, equations.
4. Explain formulas by role:
   - Define symbols.
   - Say what each term encourages or penalizes.
   - Connect the formula to the implementation location if code is available.
5. End with practical limits:
   - What the method does not solve.
   - What assumptions are expensive, brittle, or hidden.
   - What would matter for reproduction or extension.

For PDFs, use metadata and targeted extraction first. If extraction is large or noisy, chunk by page range and search method, experiment, and appendix sections instead of reading the whole PDF at once.

## Code Reading Procedure

1. Build a repository map:
   - `README*`, `docs/`, install files, environment files, `pyproject.toml`, `setup.py`, `requirements*`, `environment.yml`, `configs/`, `scripts/`, `examples/`, `tests/`.
2. Find execution entrypoints:
   - Demo/inference commands.
   - Training scripts.
   - Evaluation scripts.
   - Data preprocessing and download scripts.
   - Visualization/export scripts.
3. Trace the runtime path:
   - CLI args/config loading -> data loading -> model construction -> forward/generation -> loss/objective or sampler -> postprocess/export -> evaluation/visualization.
4. Map abstractions to research concepts:
   - Model classes, dataset classes, pipeline/orchestrator classes, renderers, optimizers, schedulers, evaluators.
5. Verify implementation depth:
   - Mark whether each paper module is implemented, delegated to a dependency/API, stubbed, missing, or only available through released assets.
6. Respect the worktree:
   - Do not modify code unless the user asks for implementation or reproduction fixes.
   - When editing is requested, keep changes narrowly scoped and verify with the project's tests or minimal commands.

## Paper-Code Alignment

Use this table when the user asks to combine paper and code:

| Paper concept | Purpose in method | Code location | Runtime evidence | Notes / gaps |
|---|---|---|---|---|
| Module or equation name | What it does | File/class/function/config | Entrypoint or call path proving use | Missing, differs, dependency, or assumption |

After the table, explain the end-to-end flow in prose:

1. What enters the system.
2. Which module transforms it first.
3. Where the central representation changes.
4. How outputs are produced.
5. Where training or optimization signals enter.
6. Which parts are paper-only, code-only, or unclear.

## Reproduction Workflow

When the user wants to reproduce or run the project:

1. Determine the minimal runnable target:
   - Smoke test, demo inference, single sample, evaluation subset, training, or full benchmark.
2. Read official setup instructions before installing or running.
3. Identify required assets:
   - Datasets, pretrained weights, API keys, licenses, generated caches, GPU/CUDA assumptions.
4. Prefer low-cost validation first:
   - `--help`, config listing, dry run, tiny input, CPU fallback, or single-sample demo.
5. Record commands and observed results:
   - Command, working directory, environment, output files, logs, errors, and next fix.
6. Be explicit about status:
   - `verified locally`, `planned but not run`, `blocked by missing asset`, `blocked by hardware`, or `paper claim not reproducible from released code`.

Do not invent expected metrics. If a benchmark is not run, say so.

## Comparative Study

For multiple papers or projects, compare on stable axes instead of isolated summaries:

- Research problem and input assumptions.
- Output representation and granularity.
- Core technical strategy.
- Data and supervision.
- Dependency on pretrained models, assets, APIs, or proprietary components.
- Code availability and implementation completeness.
- Reproduction difficulty.
- Best use case and main limitation.

If the user cares about reuse, explicitly answer: "Which method is easiest to adapt, and why?"

## Durable Notes

When the user asks to maintain notes, create or update Markdown files in the requested location. If no location is given, propose a simple local structure before writing:

```text
research_notes/
  papers/
  code_maps/
  reproduction_logs/
  glossary.md
  open_questions.md
```

Keep notes useful for future AI sessions:

- Include source paths/URLs.
- Preserve exact module and file names.
- Mark unresolved questions and unverified claims.
- Store command logs or reproduction status in a concise form.

## Default Answer Shapes

For a paper overview:

```markdown
## 一句话定位
## 问题背景和动机
## 输入输出
## 总体 Pipeline
## 核心模块
## 训练与推理
## 实验结论
## 局限和可复现性
```

For paper-code alignment:

```markdown
## 总体结论
## 论文技术路线
## 代码入口和运行路径
## 论文模块到代码映射表
## 关键实现细节
## 与论文不一致或未公开的部分
## 复现建议
```

For reproduction:

```markdown
## 当前目标
## 已确认环境和资产
## 最小可运行命令
## 预期输出
## 已运行结果
## 阻塞点和下一步
```

## Pitfalls

- Do not summarize from memory when local files are available.
- Do not treat a polished README as proof that every paper module is implemented.
- Do not conflate project setup, demo inference, training, and benchmark reproduction.
- Do not translate paper sections line by line unless the user asks for translation.
- Do not hide uncertainty; label missing code, missing assets, and unverified claims.
- Do not expand a focused learning request into a full literature survey unless the user asks.

## Completion Checklist

Before finalizing a research-study response, check:

1. The answer names the paper/repo/source material actually used.
2. The method's problem, input, output, and pipeline are clear.
3. Key modules are tied to paper evidence and, when available, code locations.
4. The explanation includes implementation or reproduction implications, not only abstract concepts.
5. Unverified claims, missing code, and reproduction blockers are explicitly marked.
6. The next learning step is concrete.
