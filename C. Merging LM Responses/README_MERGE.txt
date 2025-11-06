Merged LLM Responses – Multilingual Comparison (Folder README)
Last updated: 2025-05-05

PURPOSE
This folder stores the merged, language-aligned outputs for each model. Each merged file combines
English (High.csv), Italian (Mid.csv), and Farsi (Low.csv) responses into a single CSV so we can
compare model choices across the three languages item-by-item.

WHAT GOT MERGED (inputs come from the previous 'LLM test' step)
Base (non‑shuffled) runs:
  • GPT‑4o → High_gpt4o_new.csv, Mid_gpt4o_new.csv, Low_gpt4o_new.csv
  • Llama 3.2 → High_Llama3.2.csv, Mid_Llama3.2.csv, Low_Llama3.2.csv (name may vary, but has a column with choices)
  • Llama 3.1 70B → High_Llama3.1_70B.csv, Mid_Llama3.1_70B.csv, Low_Llama3.1_70B.csv
  • Qwen 2.5 14B → High_Qwen2.5_14B.csv, Mid_Qwen2.5_14B.csv, Low_Qwen2.5_14B.csv
  • mBERT → High_mBert.csv, Mid_mBert.csv, Low_mBert.csv
  • XLM‑R → multiple_choice_prompts_High_xlmr_output.json, ..._Mid_..., ..._Low_...

Shuffled runs (order‑effect control; only for GPT‑4o and Llama 3.1 70B):
  • GPT‑4o → "High - Shfl_Shfl_gpt4o_output.csv", "Mid - Shfl_...", "Low - Shfl_..."
  • Llama 3.1 70B → corresponding shuffled output CSVs

MERGE LOGIC (applied to every model)
1) Load the three language files for the same model.
2) Assert the same number of rows (same items) across EN/IT/FA.
3) Build a single DataFrame with parallel columns:
   - Shared metadata per language: Domain_EN/FA/IT, Prompt_EN/FA/IT, A_* … D_*
   - Model choices per language: e.g., GPT4o_EN, GPT4o_IT, GPT4o_FA
4) Save one CSV per model in this folder.

FILES YOU SEE HERE (expected)
  • Merged_GPT4o_Resp.csv                 – base GPT‑4o across EN/IT/FA
  • Merged_Llama3.2_Resp.csv              – base Llama 3.2 across EN/IT/FA
  • Merged_Llama3.1_70_Resp.csv           – base Llama 3.1 70B across EN/IT/FA
  • Merged_Qwen2.5_14B_Resp.csv           – base Qwen 2.5 14B across EN/IT/FA
  • Merged_mBert_Resp.csv                 – base mBERT across EN/IT/FA
  • Merged_XLM_R_Resp.csv                 – base XLM‑R across EN/IT/FA (converted from JSON outputs)
  • Merged_Shfl_GPT4o_Resp.csv            – shuffled GPT‑4o across EN/IT/FA
  • Merged_Shfl_Llama3.1_70B_Resp.csv     – shuffled Llama 3.1 70B across EN/IT/FA

NOTES / CONVENTIONS
- Column names for choices are model‑specific (e.g., “GPT-4o Choice”, “Llama Choice”, “XLM-R Choice”).
  During merge, we rename/assign them to consistent columns like GPT4o_EN/IT/FA, Llama_EN/IT/FA, etc.
- For XLM‑R, inputs are JSON; we read the three “*_xlmr_output.json” files and write a merged CSV.
- Encoding is UTF‑8 with BOM for Excel compatibility (utf-8-sig).
- If a file already exists, re‑running merge scripts will overwrite it with the latest results.

WHY MERGE?
Having EN/IT/FA side‑by‑side allows direct comparison of each model’s decision across translations of
the same item, which we use for cross‑lingual consistency analyses and order‑effect checks (via shuffled runs).

REPRO STEP (conceptual; see model‑specific scripts for details)
- Example (GPT‑4o base):
  Read Low_gpt4o_new.csv / Mid_gpt4o_new.csv / High_gpt4o_new.csv → verify equal length →
  assemble columns Domain_*, Prompt_*, A_*/B_*/C_*/D_* and choices → write Merged_GPT4o_Resp.csv.

CONTACT
ghanbari.haez.saba@gmail.com
