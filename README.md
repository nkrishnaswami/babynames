# babynames
ETL to prepare the SSA baby names data for a pair of Hugging Face datasets:
* https://huggingface.co/datasets/nkrishnaswami/us-ssa-baby-names-national
* https://huggingface.co/datasets/nkrishnaswami/us-ssa-baby-names-states

These replace the earlier data.world datasets, which are no longer available.

`SSA Baby Names.ipynb` downloads the latest data from the
[SSA](https://www.ssa.gov/oact/babynames/limits.html), adds a popularity rank for each
year and sex (and state), writes CSV and Parquet, and uploads both datasets.
The dataset cards are in `hf/`.
