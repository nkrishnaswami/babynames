---
license: cc0-1.0
pretty_name: US SSA Baby Names (States and Territories)
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
    data_files: namesbystate_ranks_counts.parquet
---

# US SSA Baby Names: States and Territories

Given names recorded by the US Social Security Administration for babies born in each
US state, DC and the US territories, through {{last_year}}. Each row gives the number
of births and the name's popularity rank within that state, year and sex.

The national version of this data is at
[nkrishnaswami/us-ssa-baby-names-national](https://huggingface.co/datasets/nkrishnaswami/us-ssa-baby-names-national).

## Columns

| column  | type   | description |
|---------|--------|-------------|
| `state` | string | Two-letter code: the 50 states, `DC`, `PR` (Puerto Rico) or `TR` (other territories; see below) |
| `sex`   | string | `F` or `M`, as recorded by SSA |
| `year`  | int    | Year of birth |
| `name`  | string | Given name, 2–15 characters |
| `count` | int    | Number of births with that name in that state, sex and year |
| `rank`  | int    | Popularity rank of the name within its state, year and sex; 1 is the most common |

Ranks use the "min" method for ties: names with equal counts share the
best rank, and the next rank is skipped (1, 2, 2, 4, …).
This differs from the SSA's own ranking, which numbers names by their position in
the file and breaks ties alphabetically, so ranks here may not match ssa.gov.

### Territories

`PR` and `TR` come from the SSA's separate territory files and start in 1998, while state
data starts in 1910. `TR` combines American Samoa, Guam, the Northern Mariana Islands
and the US Virgin Islands, which each have relatively few births.

## Files

- `namesbystate_ranks_counts.parquet`: the table above; this is what `load_dataset` and the viewer use
- `namesbystate_ranks_counts.csv`: the same data as CSV

```python
from datasets import load_dataset
ds = load_dataset("nkrishnaswami/us-ssa-baby-names-states")

# or, with pandas
import pandas as pd
df = pd.read_parquet("hf://datasets/nkrishnaswami/us-ssa-baby-names-states/namesbystate_ranks_counts.parquet")
```

## Source and caveats

The data comes from the SSA's
[Beyond the Top 1000 Names](https://www.ssa.gov/oact/babynames/limits.html) page
(`namesbystate.zip` and `namesbyterritory.zip`). The SSA notes:

- Only names with at least 5 occurrences in a state, year and sex are included, to
  protect privacy. Because of this, state counts do not add up to the national counts.
- Data are from Social Security card applications.
- Names are truncated to 15 characters, and the SSA does not merge spelling variants.

Ranks were computed for this dataset and are not part of the SSA files. The ETL is at
[github.com/nkrishnaswami/babynames](https://github.com/nkrishnaswami/babynames).

## License

The underlying data is a work of the US federal government and is in the public domain.
This compilation is released under CC0 1.0.
