---
license: cc0-1.0
pretty_name: US SSA Baby Names (National)
language:
  - en
tags:
  - names
  - demographics
  - social-security-administration
  - united-states
size_categories:
  - 1M<n<10M
configs:
  - config_name: default
    data_files: names_ranks_counts.parquet
---

# US SSA Baby Names: National

Every given name recorded by the US Social Security Administration for babies born
in the United States from 1880 through {{last_year}}, with the number of births and
the name's popularity rank for each year and sex.

The state-by-state version of this data is at
[nkrishnaswami/us-ssa-baby-names-states](https://huggingface.co/datasets/nkrishnaswami/us-ssa-baby-names-states).

## Columns

| column  | type   | description |
|---------|--------|-------------|
| `name`  | string | Given name, 2–15 characters |
| `sex`   | string | `F` or `M`, as recorded by SSA |
| `year`  | int    | Year of birth |
| `rank`  | int    | Popularity rank of the name within its year and sex; 1 is the most common |
| `count` | int    | Number of births with that name, sex and year |

Ranks use the "min" method for ties: names with equal counts share the
best rank, and the next rank is skipped (1, 2, 2, 4, …).
This differs from the SSA's own ranking, which numbers names by their position in
the file and breaks ties alphabetically, so ranks here may not match ssa.gov.

## Files

- `names_ranks_counts.parquet`: the table above; this is what `load_dataset` and the viewer use
- `names_ranks_counts.csv`: the same data as CSV

```python
from datasets import load_dataset
ds = load_dataset("nkrishnaswami/us-ssa-baby-names-national")

# or, with pandas
import pandas as pd
df = pd.read_parquet("hf://datasets/nkrishnaswami/us-ssa-baby-names-national/names_ranks_counts.parquet")
```

## Source and caveats

The data comes from the SSA's
[Beyond the Top 1000 Names](https://www.ssa.gov/oact/babynames/limits.html) page
(`names.zip`). The SSA notes:

- Only names with at least 5 occurrences in a year and sex are included, to protect privacy.
- Data are from Social Security card applications, so coverage of births before 1937
  (when many people applied for cards later in life) is incomplete.
- Names are truncated to 15 characters, and the SSA does not merge spelling variants.

Ranks were computed for this dataset and are not part of the SSA files. The ETL is at
[github.com/nkrishnaswami/babynames](https://github.com/nkrishnaswami/babynames).

## License

The underlying data is a work of the US federal government and is in the public domain.
This compilation is released under CC0 1.0.
