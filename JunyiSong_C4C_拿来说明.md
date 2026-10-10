# C4-C 作业自动求解 · 拿来说明

## 这是什么

一套五阶段作业求解流水线（starter kit 扩展版），本任务在线性代数上验证通过，求解率 83%。

## 环境依赖

```bash
pip install -r requirements.txt   # sympy、pyyaml 等
```

LaTeX 编译用 **tectonic**（本机已下载到工作区 `.tectonic-dl/tectonic`），无需安装 MacTeX。渲染层字符串已转英文，避免 CJK 字体缺失。

## 跑一条完整流水线

```bash
# 记得把 tectonic 所在目录加进 PATH
PATH="$工作区/.tectonic-dl:$PATH" python3 scripts/pipeline.py \
  test_cases/test3_linear_algebra.md \
  test3_output --compile
```

参数：`<输入文件> <输出目录> [--compile] [--course "数学"] [--student "姓名"]`。

## 分阶段运行（调试用）

```bash
python3 scripts/ingest.py  <输入> output/1_ingested.json
python3 scripts/parse_problems.py output/1_ingested.json output/2_parsed.json
python3 scripts/solve.py output/2_parsed.json output/3_solutions.json
python3 scripts/render_latex.py output/3_solutions.json output/homework.tex
```

## 输出产物

| 文件 | 含义 |
|---|---|
| `1_ingested.json` | 读入的原始内容 |
| `2_parsed.json` | 分题 + 分类结果 |
| `3_solutions.json` | 每题的答案/求解状态 |
| `homework.tex` | 答案排版源文件 |
| `homework.pdf` | 最终答案 PDF（需 `--compile`） |

## 关键文件位置

- 领域知识规则：`domain_skills/calculus_limits.yaml`
- 求解器实现：`scripts/solve.py`（含本次扩展的线性代数求解器）
- 分类逻辑：`scripts/classify.py`
- 测试集：`test_cases/test1_*.md`（切线/ε-δ）、`test2_limits.md`（极限）、`test3_linear_algebra.md`（线性代数）
