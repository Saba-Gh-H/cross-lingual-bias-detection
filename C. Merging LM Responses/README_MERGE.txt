MERGING MULTILINGUAL RESPONSES

This folder contains eight Jupyter notebooks. Generated merged CSVs are not
checked in. Each notebook combines outputs for English (High), Italian (Mid),
and Persian (Low) by row position. Preserve and verify scenario alignment.

NOTEBOOK -> OUTPUT
Merged_GPT4o_Resp.ipynb -> GPT4o_Merged_Multilingual.csv
Merged_Llama3.1_70_Resp.ipynb -> LLaMA3.1_70_Merged_Multilingual.csv
Merged_Llama3.2_Resp.ipynb -> LLaMA3.2_Merged_Multilingual.csv
Merged_Qwen2.5_14B_Resp.ipynb -> Qwen2.5_Merged_Multilingual.csv
Merged_mBert_Resp.ipynb -> mBERT_Merged_Multilingual.csv
Merged_XLM_R_Resp.ipynb -> XLM-R_Merged_Multilingual.csv
Merged_Shfl_GPT4o_Resp.ipynb -> GPT4o_Shuffled_Merged_Multilingual.csv
Merged_Shfld_Llama3.1_70B_Resp.ipynb -> LLaMA3.1_Shuffled_Merged_Multilingual.csv

WORKING DIRECTORY AND INPUTS
Use the shared A. Datasets working directory described in
../docs/REPRODUCIBILITY.md. Inputs are model-generated CSVs, except mBERT and
XLM-R which read generated JSON. That guide gives the complete filename map.

Two base Llama input patterns do not match inference filenames:
- Llama 3.1 writes *_llama_output_new.csv; merge expects *_llama3.1_70_output.csv.
- Llama 3.2 writes *_llama_output_3.2.csv; merge expects *_llama3.2_output.csv.
Adjust the merge input paths or copy outputs under the expected names.

OUTPUT CONVENTIONS
Outputs contain parallel language columns such as Domain_EN/IT/FA,
Prompt_EN/IT/FA, and model-specific choice fields. Not all merge notebooks
retain all option text fields. The XLM-R choice prefix is XLMR.
Files are written with utf-8-sig encoding; rerunning can overwrite them.
Returned control-run letters remain presented labels; the merge does not
map them back to original trait identities for original/control comparisons.

AUTHORSHIP, LICENSE, AND CITATION
Original code: Saba Ghanbari Haez (sole author).
See ../LICENSE, ../NOTICE, and ../CITATION.bib.
Paper credit is separate: ../docs/PAPER_CITATION.md.

CONTACT
ghanbari.haez.saba@gmail.com
