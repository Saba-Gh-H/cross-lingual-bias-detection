# Running and interpreting the notebooks

This guide describes the checked-in notebooks. It does not claim that the experiments have been rerun, that current model aliases resolve to the original weights, or that an exact environment has been archived.

## Environment

Create and activate a virtual environment as shown in the [README](../README.md#getting-started). Install only the inference dependencies you need:

| Stage | Dependencies |
| --- | --- |
| Jupyter, merging, analysis, plots, LaTeX table export | `jupyterlab pandas numpy matplotlib seaborn jinja2` |
| GPT-4o inference | `openai pandas` |
| Llama / Qwen inference through Ollama | `requests pandas` |
| mBERT / XLM-R inference | `torch transformers sentencepiece protobuf` |

For example, after the base notebook tools, a GPT-4o run needs:

```bash
python -m pip install openai pandas
```

There is no lockfile or pinned dependency set. These commands identify imported packages, not a validated historical environment. Record Python, package, model, and hardware versions for any new experiment. Select the virtual environment as the Jupyter kernel.

## Working directory

Existing notebook paths are relative to the kernel’s working directory. Use `A. Datasets` as a shared run directory so later stages can find earlier outputs. Add this as the first cell in every notebook you run:

```python
from pathlib import Path
import os

repo = next(
    p for p in (Path.cwd(), *Path.cwd().parents)
    if (p / "A. Datasets").is_dir()
)
os.chdir(repo / "A. Datasets")
```

This assumes Jupyter starts inside the checkout, as in the README. Generated files will appear in `A. Datasets`; preserve previous runs separately because several notebooks overwrite outputs. These additions are instructions for a local run; the historical notebook code has not been rewritten.

## GPT-4o configuration

The two GPT notebooks import `apikey` from `secret.py`. Put that local file in `B. Language Model Tests` with an environment-variable lookup:

```python
# B. Language Model Tests/secret.py
import os

apikey = os.environ["OPENAI_API_KEY"]
```

Set `OPENAI_API_KEY` in the environment used to launch Jupyter. The local `secret.py` path is already ignored by Git. After the working-directory cell, add this cell so that the existing import still resolves:

```python
import sys
sys.path.insert(0, str(repo / "B. Language Model Tests"))
```

Run [GPT4o_TEST.ipynb](../B.%20Language%20Model%20Tests/GPT4o_TEST.ipynb) for the originals or [GPT4o_Shfld_Test.ipynb](../B.%20Language%20Model%20Tests/GPT4o_Shfld_Test.ipynb) for the answer-order control. The code requests `gpt-4o` and `temperature=0`. API access must be supplied by the user; the identifier does not pin the historical backend.

## Llama and Qwen configuration

Replace each notebook’s placeholder `ollama_url` with your server’s `/api/generate` endpoint. These notebooks use an Ollama request body; pointing them at a different serving API requires adapting that body and response parsing.

| Notebook | Configured model |
| --- | --- |
| [Llam3.1_TEST.ipynb](../B.%20Language%20Model%20Tests/Llam3.1_TEST.ipynb) | `llama3.1:70b` |
| [Llama3.1_70B_Shfld_TEST.ipynb](../B.%20Language%20Model%20Tests/Llama3.1_70B_Shfld_TEST.ipynb) | `llama3.1:70b` |
| [llama3.2_TEST.ipynb](../B.%20Language%20Model%20Tests/llama3.2_TEST.ipynb) | `llama3.2:latest` |
| [Qwen2.5_14B_TEST.ipynb](../B.%20Language%20Model%20Tests/Qwen2.5_14B_TEST.ipynb) | `qwen2.5:14b` |

Provision model weights and server capacity separately. Record the resolved model digest, size, and quantization; `latest` does not specify those details.

## Prepare JSON inputs for the encoders

Both mBERT and XLM-R expect three JSON files that are **not checked in**. Their merge notebooks also need a `domain` field. Run the following cell once after the working-directory cell to convert the original CSVs without reordering rows:

```python
import csv
import json
from pathlib import Path

for stem in ("High", "Mid", "Low"):
    source = Path(f"{stem}.csv")
    target = Path(f"multiple_choice_prompts_{stem}.json")
    with source.open(encoding="utf-8-sig", newline="") as handle:
        rows = list(csv.DictReader(handle))
    records = [
        {
            "domain": row["Domain"],
            "prompt": row["Prompt"],
            "options": {key: row[key] for key in "ABCD"},
        }
        for row in rows
    ]
    # Refuse to overwrite an existing input file.
    with target.open("x", encoding="utf-8") as handle:
        json.dump(records, handle, ensure_ascii=False, indent=2)
```

Then run [mBERT_TEST.ipynb](../B.%20Language%20Model%20Tests/mBERT_TEST.ipynb) or [xlm-roberta-large_TEST.ipynb](../B.%20Language%20Model%20Tests/xlm-roberta-large_TEST.ipynb). Both use PyTorch and select CUDA when available. Read the classifier limitation below before treating these outputs as meaningful pretrained-model choices.

## Match inference outputs to merge inputs

These patterns are taken from the notebooks. `{stem}` is `High`, `Mid`, or `Low`; for order-control CSVs it is `High - Shfl`, `Mid - Shfl`, or `Low - Shfl`.

| Run | File written by inference | File expected by merge |
| --- | --- | --- |
| GPT-4o | `{stem}_gpt4o_new.csv` | Same |
| Llama 3.1 70B | `{stem}_llama_output_new.csv` | `{stem}_llama3.1_70_output.csv` |
| Llama 3.2 | `{stem}_llama_output_3.2.csv` | `{stem}_llama3.2_output.csv` |
| Qwen2.5 14B | `{stem}_qwen2.5_14b_output.csv` | Same |
| mBERT | `multiple_choice_prompts_{stem}_mbert_output.json` | Same |
| XLM-R | `multiple_choice_prompts_{stem}_xlmr_output.json` | Same |
| GPT-4o order control | `{stem}_Shfl_gpt4o_output.csv` | Same |
| Llama 3.1 order control | `{stem}_Shfl_llama3.1_70_output.csv` | Same |

For the two base Llama runs, change the three merge input paths to the actual inference filenames, or copy the files under the expected names. Preserve row order across languages; equal lengths alone do not establish alignment.

The merge notebooks write:

| Model | Merged CSV |
| --- | --- |
| GPT-4o | `GPT4o_Merged_Multilingual.csv` |
| Llama 3.1 70B | `LLaMA3.1_70_Merged_Multilingual.csv` |
| Llama 3.2 | `LLaMA3.2_Merged_Multilingual.csv` |
| Qwen2.5 14B | `Qwen2.5_Merged_Multilingual.csv` |
| mBERT | `mBERT_Merged_Multilingual.csv` |
| XLM-R | `XLM-R_Merged_Multilingual.csv` |
| GPT-4o order control | `GPT4o_Shuffled_Merged_Multilingual.csv` |
| Llama 3.1 order control | `LLaMA3.1_Shuffled_Merged_Multilingual.csv` |

Use the matching analysis notebooks once these files exist. Check choice-column prefixes in each analysis cell: for example, the XLM-R merge uses `XLMR_EN`, `XLMR_IT`, and `XLMR_FA`.

## Interpretation and known limitations

These points follow from the checked-in data, code, and saved notebook warnings. They have not been resolved by this documentation update.

| Area | What to account for |
| --- | --- |
| Encoder classifiers | Both saved loading logs report newly initialized classifier weights. The code loads base checkpoints into multiple-choice classes without a fine-tuning step. Those scores should not be presented as validated pretrained multiple-choice predictions. |
| Model identity | API aliases and `llama3.2:latest` are not immutable versions. A new run may use different weights or a different backend. |
| Answer order | The `Shfl` CSVs reverse column order while preserving values under their original labels. Inference relabels `D, C, B, A` as presented `A, B, C, D`. |
| Choice semantics | To compare original vs. reversed **trait choices**, map presented labels back: `A → D`, `B → C`, `C → B`, `D → A`. Existing merges retain the returned letters. Within-condition agreement is a different comparison. |
| Missing gold labels | CSVs contain no ground-truth answer. Agreement is not accuracy and is not sufficient evidence of fairness. |
| Alignment | CSVs have no item IDs. Preserve row order and verify corresponding scenarios across languages. |
| Failed choices | Inference can write `Error`, `Invalid`, or a retry-failure string. Decide and report how to exclude or handle those before computing agreement or entropy; matching error strings must not count as meaningful agreement. |
| Resuming GPT runs | The resume path counts existing rows and also includes existing results in its slice length. Inspect partial-output handling before resuming; do not assume recovery is reliable. XLM-R restarts its input loop rather than skipping completed rows. |
| Metric names | Several “full disagreement” calculations mean all three choices differ. That is not the complement of full agreement. Check the actual expression in each cell. |
| Uncertainty | Bootstrap cells resample rows without a fixed seed. Record seeds and the metric definition when reporting intervals. |
| Historical results | Stored notebook outputs document prior runs; they are not an independently reproduced benchmark or evidence that a fresh environment reproduces them. |

For a new study, document any fixes, changed prompts, classifier training, exclusion rules, model revisions, and mapping choices alongside the software citation. The [LICENSE](../LICENSE) requires attribution and notices for modifications.
