Agreement / Disagreement Analysis – README
Last updated: 2025-05-05

PURPOSE
This folder contains scripts and outputs that quantify cross-lingual agreement and disagreement
for each model using the merged, language-aligned files created in the previous step.
All metrics compare choices across EN (High), IT (Mid), and FA (Low) for the *same* item.

INPUTS (produced in the Merged folder)
One merged file per model, e.g.:
  • GPT4o_Merged_Multilingual.csv
  • LLaMA3.2_Merged_Multilingual.csv
  • LLaMA3.1_70_Merged_Multilingual.csv
  • Qwen2.5_Merged_Multilingual.csv
  • mBERT_Merged_Multilingual.csv
  • XLM-R_Merged_Multilingual.csv
(Names may vary slightly; each contains columns like <MODEL>_EN, <MODEL>_FA, <MODEL>_IT.)

WHAT WE COMPUTE
1) Full Agreement Rate
   For a given model prefix (e.g., GPT4o), we mark an item as “Full_Agreement” when:
       <MODEL>_EN == <MODEL>_FA == <MODEL>_IT
   We then report the % of items with full agreement.

2) Disagreement Types
   Items without full agreement are categorized as:
     • FA diverged – EN == IT ≠ FA
     • IT diverged – EN == FA ≠ IT
     • EN diverged – FA == IT ≠ EN
     • All different – all three choices are different
     • Other – a fallback label if none of the above patterns matched

3) Domain-wise Disagreement
   We count disagreements grouped by Domain (using Domain_EN).

4) Option Frequencies (per language)
   Example shown for FA: distribution of A/B/C/D chosen by the model on the FA rows.
   (Repeatable for EN/IT if needed.)

5) Entropy of Choices (per language)
   To quantify response diversity, we compute Shannon entropy of the choice distribution
   separately for EN, FA, and IT. Higher entropy = more evenly spread choices.
   Example snippet:
       def compute_entropy(series):
           counts = series.value_counts(normalize=True)
           return -np.sum(counts * np.log2(counts))
       entropy_fa = compute_entropy(df["GPT4o_FA"])
       entropy_it = compute_entropy(df["GPT4o_IT"])
       entropy_en = compute_entropy(df["GPT4o_EN"])

6) Confidence Intervals (optional, via bootstrap)
   • Agreement CI: resample items with replacement N times (default 1000) and compute the
     distribution of full-agreement rates → report mean and 95% CI.
   • Disagreement CI: same procedure on the complement (1 − agreement).
   Functions provided:
     - bootstrap_agreement_confidence_interval(filepath, model_prefix, num_bootstrap=1000, confidence=0.95)
     - bootstrap_disagreement_confidence_interval(filepath, model_prefix, num_bootstrap=1000, confidence=0.95)

7) Total Summary Across Models
   A single table (CSV) that aggregates, for each model:
     • Agreement %
     • Count of each Disagreement_Type
     • Option distribution in FA (A/B/C/D %)
     • (Entropy values can be added here if desired)
   Saved as: LLM_Multilingual_Agreement_Summary.csv

SHUFFLED VS. BASE COMPARISONS
We also analyze order effects for the models that have shuffled runs:
  • GPT-4o and LLaMA-3.1-70B only.
  Inputs: the corresponding “Merged_Shfl_*_Resp.csv” files.
  Outputs in this folder include files like:
    - DisAgreement_CompareShfl_GPT4o
    - DisAgreement_CompareShfl_Llama3.1_70B
  These compare disagreement rates and patterns between base and shuffled items.

EXPECTED FILES IN THIS FOLDER (examples)
  • DisAgreement_Rate_GPT4o
  • DisAgreement_Rate_Llama3.1_70
  • DisAgreement_Rate_Llama3.2
  • DisAgreement_Rate_Qwen2.5_14B
  • DisAgreement_Rate_mBert
  • DisAgreement_Rate_XLM_R
  • DisAgreement_Rate_Shfl_GPT4o
  • DisAgreement_Rate_Shfld_Llama3.1_Resp
  • DisAgreement_CompareShfl_GPT4o
  • DisAgreement_CompareShfl_Llama3.1_70B
  • DisAgreement_Rate_Total
  • Domain disagreement all
(Names reflect the scripts/exports you used; content corresponds to the computations above.)

CODE SNIPPETS USED (summary)
- Full agreement and domain breakdown (per model file):
    df = pd.read_csv("GPT4o_Merged_Multilingual.csv")
    df["Full_Agreement"] = (df["GPT4o_EN"] == df["GPT4o_FA"]) & (df["GPT4o_EN"] == df["GPT4o_IT"])
    agreement_pct = df["Full_Agreement"].mean() * 100

- Entropy (per language column):
    entropy = compute_entropy(df["GPT4o_FA"])  # repeat for EN/IT

- Bootstrap confidence intervals (agreement/disagreement):
    bootstrap_agreement_confidence_interval("GPT4o_Merged_Multilingual.csv", model_prefix="GPT4o")
    bootstrap_disagreement_confidence_interval("GPT4o_Merged_Multilingual.csv", model_prefix="GPT4o")

- Total summary across models:
    Builds a table by looping over the merged files for GPT-4o, LLaMA (3.2, 3.1-70B), mBERT, XLM-R, Qwen 2.5,
    then writes LLM_Multilingual_Agreement_Summary.csv.

DEPENDENCIES
  pip install pandas numpy

NOTES
- All computations assume row alignment across languages within each merged CSV.
- For XLM-R, inputs are JSON at the model-inference stage, but are already merged into CSV at this step.
- If file names differ from the examples, adjust the script paths and model_prefix strings accordingly.

CONTACT & LICENSE
Replace with your contact and license information if these analysis outputs will be shared.
