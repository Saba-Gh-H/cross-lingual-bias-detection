LLM Evaluation – Adjective Selection (Folder README)
Last updated: 2025‑05‑05

OVERVIEW
This folder contains scripts to run multiple LLMs on the adjective–selection task. Each model reads the language datasets (High.csv = English, Mid.csv = Italian, Low.csv = Farsi) and outputs a single choice (A/B/C/D) per item. Shuffled variants test order effects.

WHAT’S IN THIS FOLDER (typical)
- GPT4o_TEST.py / GPT4o_Shfld_TEST.py        Run GPT‑4o on base and shuffled CSVs
- Llama3.*_TEST.py / Llama3.*_Shfld_TEST.py  Run local/remote Llama on base/shuffled
- Qwen*_TEST.py                               Run local/remote Qwen on base/shuffled
- mBERT_TEST.py                               Run mBERT locally (Hugging Face)
- xlm-roberta-large_TEST.py                   Run XLM‑Roberta locally (Hugging Face)
(Names can vary slightly; pair “_Shfld_” scripts with the “- Shfl.csv” files.)

INPUT DATA
- Base CSVs: High.csv (EN), Mid.csv (IT), Low.csv (FA)
- Shuffled CSVs: High - Shfl.csv, Mid - Shfl.csv, Low - Shfl.csv
Required columns in CSVs:
  Domain, Prompt, A, B, C, D
Outputs are saved next to each input file.

COMMON OUTPUTS
- Base runs → “*_gpt4o_new.csv” or similar, with an added column like “GPT-4o Choice”
- Shuffled runs → “*_Shfl_gpt4o_output.csv” (or model-specific naming)
Scripts append a new column and save incrementally (safe to resume).

GPT‑4o (OpenAI) — HOW TO RUN
1) Python deps:
   pip install pandas openai
2) Create secret.py next to the script with:
   apikey = "YOUR_OPENAI_API_KEY"
3) The script sets:
   gpt_model = "gpt-4o"
   It uses a strict system prompt and temperature=0. It retries on rate limits.
4) Run the script; it will iterate over High.csv, Mid.csv, and Low.csv and produce
   “High_gpt4o_new.csv”, etc.
5) Shuffled version:
   - Use the *_Shfld_TEST script. It remaps shuffled columns back to A–D for the prompt
     and writes e.g. “High - Shfl_Shfl_gpt4o_output.csv” (exact name may vary in your copy).

LLAMA (LOCAL/REMOTE SERVER) — HOW TO RUN
- These scripts call your own LLM server (e.g., text-generation-webui, vLLM, Ollama, or custom REST).
- Replace the placeholder endpoint/base URL with your own server address and ensure the route,
  headers, and model name match your deployment.
- Everything else mirrors the GPT flow: read CSV → build prompt → return A/B/C/D → save outputs.
- Do the same for shuffled runs using the corresponding *_Shfld_* script.

QWEN (LOCAL/REMOTE SERVER) — HOW TO RUN
- Same as Llama: replace the server URL and (if needed) model name.
- Keep temperature at 0 and return only A/B/C/D.

mBERT — HOW TO RUN (LOCAL HF)
- Install: pip install torch transformers
- Script loads a multilingual BERT (mBERT) checkpoint and runs multiple choice locally.
- No API keys/endpoints required; run the script as-is.
- Output format matches the other models.

XLM‑Roberta — HOW TO RUN (LOCAL HF)
- Install: pip install torch transformers
- Script loads `xlm-roberta-large` and runs multiple choice locally.
- Input format: JSON, not CSV. It expects files named:
    multiple_choice_prompts_Low.json
    multiple_choice_prompts_Mid.json
    multiple_choice_prompts_High.json
  Each item has: { "prompt": "... [ ] ...", "options": {"A": "...", "B": "...", "C": "...", "D": "..."} }
- Output: the script writes a sibling file with “_xlmr_output.json” and adds “XLM-R Choice”.

NOTE ABOUT THE XLM-R NAME
- The folder shows “xlm-roberta-large_TEST”, while the model code uses model_name="xlm-roberta-large".
  That’s expected: the script filename includes “_TEST”, but the Hugging Face model identifier
  is simply “xlm-roberta-large”. No change required unless you want a different checkpoint.

PROMPTING & INSTRUCTIONS (ALL MODELS)
System/user prompt template:
  - Show the sentence with placeholder “[ ]”.
  - Provide options labeled A, B, C, D.
  - Instruct the model to answer with ONLY the single letter.
We use temperature=0 to ensure deterministic choices when supported.

SHUFFLED FILES
- Shuffled CSVs permute the options’ order per item to test order effects.
- Example mapping used in GPT Shfl script (adjust if your shuffled columns differ):
    A ← D,  B ← C,  C ← B,  D ← A
  The script ensures the LLM still sees options labeled A–D in the prompt.
- Outputs are saved incrementally and can resume mid-file.

RESUME LOGIC
- If an output CSV/JSON exists, scripts read it, count finished rows, and continue from there.
- They always rewrite a complete slice up to the current row to keep progress if interrupted.

ERROR & RATE-LIMIT HANDLING
- GPT‑4o: catches openai.RateLimitError and retries (exponential-ish backoff); other exceptions are logged.
- Local HF models: exceptions per item are logged and the choice is set to “Error”.

ENVIRONMENT
- Python ≥ 3.9 recommended.
- Install as needed:
    pip install pandas openai torch transformers
- GPU is optional for HF models but recommended; scripts auto‑detect CUDA.

SECRETS & ENDPOINTS
- OpenAI: keep your API key in secret.py (never commit it).
- Llama/Qwen: store your base URL and any tokens in environment variables or a local config.

CONTACT & LICENSE
- Replace this line with your contact and dataset/model usage license details.
