Adjective Selection – Multilingual LLM Study
Last updated: 2025-05-05

OVERVIEW
This repository organizes the full workflow of our study on adjective selection across three languages:
  • English (High)
  • Italian (Mid)
  • Farsi (Low)

We evaluate multiple LLMs (GPT‑4o, LLaMA variants, Qwen, mBERT, XLM‑R) on parallel datasets and analyze
their cross‑lingual consistency, order effects, and disagreement patterns.

FOLDER STRUCTURE
1. Dataset
   Contains the base CSV files (High/Mid/Low) and their shuffled versions.
   → These are the inputs for all model evaluations.

2. LLM Test
   Scripts for running each model on the datasets.
   → GPT‑4o uses OpenAI API (apikey required).
   → LLaMA/Qwen run via local server endpoints.
   → mBERT/XLM‑R run locally with Hugging Face.

3. Merged
   Combines model outputs across EN/IT/FA into one CSV per model.
   → Enables direct cross‑lingual comparison item‑by‑item.

4. Agreement / Disagreement
   Analyses agreement rates, disagreement types, option distributions, domain effects,
   shuffled vs. base comparisons, and entropy of choices.
   → Includes bootstrapped confidence intervals and summary tables across models.

HOW TO USE
- Start from **Dataset**, run the appropriate **LLM Test** scripts.
- Merge outputs into aligned CSVs (**Merged**).
- Run the analysis scripts in **Agreement / Disagreement** to compute agreement, disagreement,
  entropy, and cross‑model/domain summaries.

CONTACT 
ghanbari.haez.saba@gmail.com
