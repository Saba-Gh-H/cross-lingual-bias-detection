Dataset: Adjective Selection Across Languages
Folder: /datasets
Last updated: 2025-05-05

OVERVIEW
This folder contains the CSV datasets used to study adjective selection across three languages. 
Each language has a base dataset and a variant with shuffled answer options. 
All files use the same item set; only the ordering of the options differs between the base and shuffled versions.

FILE MAP
- Low.csv              → Farsi (fa)
- Mid.csv              → Italian (it)
- High.csv             → English (en)
- Low - Shfl.csv       → Farsi with options shuffled
- Mid - Shfl.csv       → Italian with options shuffled
- High - Shfl.csv      → English with options shuffled

INTENDED USE
Use the base files (Low/Mid/High) when you need the original option ordering.
Use the “- Shfl” files to control for order effects; each item’s option list is randomly permuted, 
but the item content and the gold label remain the same as in the corresponding base file.

SCHEMA
All CSVs share the same columns (field names may vary depending on your export; adapt as needed):
- item_id            Unique identifier for the item.
- prompt/sentence    The sentence or context containing the target slot.
- target_slot        Marker or index indicating where the adjective choice applies.
- options            Candidate adjectives (either as multiple columns like option_a/option_b/... 
                     or as a single list field).
- correct_label      The key identifying the correct option (e.g., the text, index, or label).
- language           Language code for the row (optional; implied by file if omitted).
- split              Data split if used (e.g., train/dev/test), otherwise empty.

NOTES ON SHUFFLED FILES
- Items are 1:1 with the base files (same item_id).
- Only the ordering of options is changed.
- correct_label always points to the correct option after shuffling (i.e., the label/text tracks the shuffle).

FORMATS & CONVENTIONS
- Encoding: UTF-8 (recommended). If opening in Excel, ensure UTF-8 to preserve non-ASCII characters.
- Delimiter: comma. Quoted fields may contain commas.
- Line endings: LF.
- Missing values: empty string.

LOADING EXAMPLE (Python / pandas)
import pandas as pd
en = pd.read_csv("High.csv")
en_shfl = pd.read_csv("High - Shfl.csv")
# Join on item_id if you need to compare base vs. shuffled:
merged = en.merge(en_shfl, on="item_id", suffixes=("_base", "_shfl"))

QUALITY CHECKS (optional but recommended)
- Verify that each “- Shfl” file has the same number of rows and the same set of item_id values as its base file.
- Confirm that correct_label points to the correct option after shuffling.

LICENSE & CITATION
If you use these data, please cite the accompanying paper/report for this study.


CONTACT
For questions or issues about the dataset format or contents, contact: [Saba Ghanbari Haez / ghanbari.haez.saba@gmail.com]
