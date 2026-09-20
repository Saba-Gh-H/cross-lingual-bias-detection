DATASETS: MULTILINGUAL TRAIT ATTRIBUTION

FILE MAP
High.csv          English (en)
Mid.csv           Italian (it)
Low.csv           Persian / Farsi (fa)
High - Shfl.csv   English answer-order control
Mid - Shfl.csv    Italian answer-order control
Low - Shfl.csv    Persian answer-order control

ACTUAL SCHEMA
Each file has 60 rows across 12 domains.
Original columns: Domain, Prompt, A, B, C, D.
Order-control columns: Domain, Prompt, D, C, B, A.
Prompt contains the scenario; A-D contain adjective-pair options.
There is no item_id, correct_label, split, or gold-answer column.
Use UTF-8 (utf-8-sig also handles a possible byte-order mark).

ALIGNMENT AND ANSWER ORDER
Preserve row order across languages and between original/control files.
Equal row counts alone do not prove that scenarios are aligned.
The Shfl files retain the original values under the same named option columns;
only the column order is reversed. The control inference notebooks relabel
D, C, B, A as the presented A, B, C, D. This is a fixed reversal, not independent
random shuffling for every row. Map returned labels back to original options
before comparing trait choices across original and control conditions.

LOADING EXAMPLE (from this directory)
import pandas as pd
en = pd.read_csv("High.csv", encoding="utf-8-sig")
assert len(en) == 60
assert list(en.columns) == ["Domain", "Prompt", "A", "B", "C", "D"]

ENCODER INPUTS
mBERT and XLM-R expect JSON inputs that are not checked in. A conversion cell
in ../docs/REPRODUCIBILITY.md preserves domain, prompt, option labels, and rows.

LICENSE AND CITATION
See ../LICENSE for permissions covering only rights the licensor owns or is
authorized to license. This does not assert sole dataset authorship.
Cite the repository using ../CITATION.bib; cite the manuscript separately when
using its methodology or findings (../docs/PAPER_CITATION.md).

CONTACT
ghanbari.haez.saba@gmail.com
