MODEL INFERENCE NOTEBOOKS

All executable research files in this folder are .ipynb notebooks, not .py
scripts. Start with ../README.md and ../docs/REPRODUCIBILITY.md.

NOTEBOOKS AND CONFIGURED MODELS
GPT4o_TEST.ipynb                  gpt-4o (OpenAI API)
GPT4o_Shfld_Test.ipynb            gpt-4o, reversed answer presentation
Llam3.1_TEST.ipynb                llama3.1:70b (Ollama)
Llama3.1_70B_Shfld_TEST.ipynb      llama3.1:70b, reversed answer presentation
llama3.2_TEST.ipynb               llama3.2:latest (Ollama; revision not pinned)
Qwen2.5_14B_TEST.ipynb            qwen2.5:14b (Ollama)
mBERT_TEST.ipynb                 bert-base-multilingual-cased (Transformers)
xlm-roberta-large_TEST.ipynb      xlm-roberta-large (Transformers)

INPUTS AND SETUP
GPT/Llama/Qwen read High.csv, Mid.csv, Low.csv or their Shfl counterparts.
Columns: Domain, Prompt, A, B, C, D (Shfl files reverse column order).
Use the working-directory cell in the setup guide to find A. Datasets.
GPT notebooks import apikey from a local secret.py. The guide shows how to
read the key from the environment while keeping the existing notebook import.
Llama/Qwen require a configured Ollama /api/generate endpoint and model weights.

Both encoder notebooks read multiple_choice_prompts_High.json, plus Mid and
Low equivalents. These JSON inputs are absent; use the guide's conversion cell.
Each record needs domain, prompt, and options (an A-D mapping).

OUTPUTS AND LIMITATIONS
Inference writes a choice column to CSV or a choice field to JSON.
The setup guide lists actual output names and the two base Llama merge-name
mismatches. Do not assume every notebook resumes reliably after interruption.

The encoder code uses BertForMultipleChoice / XLMRobertaForMultipleChoice.
Saved warnings report newly initialized classifier weights; no fine-tuning
step is included. These are not masked-token likelihood scoring notebooks.

Preserve row alignment, validate A-D responses, and document how failed model
calls are handled. For original/control trait comparisons, map reversed
presentation labels back to the original option identities.

AUTHORSHIP, LICENSE, AND CITATION
Original code: Saba Ghanbari Haez (sole author).
See ../LICENSE, ../NOTICE, ../CITATION.cff, and ../CITATION.bib.
Paper credit is separate: ../docs/PAPER_CITATION.md.

CONTACT
ghanbari.haez.saba@gmail.com
