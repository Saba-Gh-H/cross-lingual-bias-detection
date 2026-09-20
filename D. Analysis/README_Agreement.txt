CROSS-LANGUAGE AGREEMENT AND DISAGREEMENT

This folder contains Jupyter notebooks using the merged CSVs from stage C.
Read ../README.md and ../docs/REPRODUCIBILITY.md for setup and file mappings.
Generated CSVs are not checked in; saved notebook outputs are historical runs.

NOTEBOOK GROUPS
DisAgreement_Rate_*.ipynb          Per-model metrics and overall summary
DisAgreement_CompareShfl_*.ipynb   Original/reversed presentation comparisons
Domain disagreement all.ipynb     Domain-level summaries
Entropy of choice_All.ipynb        Shannon entropy of option frequencies

METRICS
- Full agreement: EN == IT == FA for the same scenario.
- One-language divergence: two languages agree, the third differs.
- All different: all three languages select different options.
- Domain counts and per-language option frequencies.
- Shannon entropy: concentration/diversity of choices, using base-2 logs.
- Bootstrap intervals: uncertainty from resampling scenarios.

IMPORTANT DISTINCTIONS
Several functions named "full disagreement" measure all three choices being
different. This is not 1 - full agreement: one-language divergence also fails
full agreement. Check the actual formula and column prefix in each cell.
Agreement measures consistency; no gold labels are provided for accuracy.

BEFORE ANALYSIS
Use the shared working directory described in the setup guide. Verify that
rows correspond across languages and that choices are valid A-D letters.
Specify how Error, Invalid, missing, or failed responses are treated; matching
failure strings should not count as meaningful model agreement.

For original/control trait comparisons, reverse the control mapping:
presented A -> original D, B -> C, C -> B, D -> A.
Existing merged files retain presented letters. Within-condition agreement
and between-condition trait agreement answer different questions.

The setup guide also documents untrained encoder classifier heads, unpinned
model identities, and unseeded bootstrap sampling. Account for these when
interpreting results or preparing a new study.

AUTHORSHIP, LICENSE, AND CITATION
Original code: Saba Ghanbari Haez (sole author).
See ../LICENSE, ../NOTICE, and ../CITATION.bib.
Paper credit is separate: ../docs/PAPER_CITATION.md.

CONTACT
ghanbari.haez.saba@gmail.com
