# ICDMap

[![PyPI version](https://img.shields.io/pypi/v/icdmap)](https://pypi.org/project/icdmap/)
[![Python versions](https://img.shields.io/pypi/pyversions/icdmap)](https://pypi.org/project/icdmap/)
[![License](https://img.shields.io/pypi/l/icdmap)](https://github.com/Brritany/ICDMAP/blob/main/LICENSE)
[![Downloads](https://static.pepy.tech/badge/icdmap)](https://pepy.tech/project/icdmap)
[![Downloads/month](https://static.pepy.tech/badge/icdmap/month)](https://pepy.tech/project/icdmap)
[![Open issues](https://img.shields.io/github/issues/Brritany/ICDMAP)](https://github.com/Brritany/ICDMAP/issues)
[![Stars](https://img.shields.io/github/stars/Brritany/ICDMAP)](https://github.com/Brritany/ICDMAP/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/Brritany/ICDMAP)](https://github.com/Brritany/ICDMAP)
[![Status](https://img.shields.io/badge/status-active-brightgreen.svg)](https://github.com/Brritany/ICDMAP)

A command-line and Python library for converting ICD codes using an offline SQLite database.

## Features

- ✅ Offline lookup via included `icd_mapping.db` and `death_cause_mapping.db`
- ✅ CLI and Python API support
- ✅ Structured JSON output with Chinese/English descriptions and mapping code
- ✅ Supports ICD-9 → ICD-10 and ICD-10 → ICD-9
- ✅ Auto-detect or manual specification of code type
- ✅ Supports cause-of-death mapping by ICD code or Cause code

## Usage

### 1. CLI

```bash
# Single code (auto-detect type)
icdmap 250.00

# Force as ICD-9 input
icdmap 250.00 --source icd9

# Force as ICD-10 input
icdmap I10 --source icd10

# Batch from CSV with column 'ICD9_CODE'
icdmap --csv input.csv -o output.json

# Batch with manual source type
icdmap --csv input.csv --source icd9 -o output.json

# Death cause mapping by ICD-9
icdmap 410 --mode death --source icd9

# Death cause mapping by CSV
icdmap --csv death.csv --mode death -o death_output.json
```

### 2. Python API

```python
from icdmap import ICDConverter, DeathCauseMapper

# ICD mapping
mapper = ICDConverter()
mapper.convert("250.00")
mapper.convert(["250.00", "I10"])

# Death cause mapping
death_mapper = DeathCauseMapper()
death_mapper.convert("41401", source="icd9")
death_mapper.convert(["41071", "431"], source="icd9")
```

### 3. Batch Mapping with Pandas (and tqdm progress)

```python
import pandas as pd
from icdmap import ICDConverter, DeathCauseMapper

# Create sample data (simulate ICD-9 code)
df = pd.DataFrame({
    "ICD9_CODE": ["250.00", "41401", "0389", "431", "41071"]
})

# ICD mapping
icd_mapper = ICDConverter()
icd_results = icd_mapper.convert(df["ICD9_CODE"])

def get_icd_field(code, field):
    return icd_results[code][0][field] if icd_results[code] else pd.NA

df["ICD-10"] = df["ICD9_CODE"].map(lambda x: get_icd_field(x, "ICD-10"))
df["ICD-10 English"] = df["ICD9_CODE"].map(lambda x: get_icd_field(x, "ICD-10 English"))
df["ICD-9 English"] = df["ICD9_CODE"].map(lambda x: get_icd_field(x, "ICD-9 English"))

# Death cause mapping
death_mapper = DeathCauseMapper()
death_results = death_mapper.convert(df["ICD9_CODE"], source="icd9")

def get_death_field(code, field):
    return death_results[code][0][field] if death_results[code] else "Other causes"

df["CAUSE_CN"] = df["ICD9_CODE"].map(lambda x: get_death_field(x, "Cause_Chinese"))
df["CAUSE_EN"] = df["ICD9_CODE"].map(lambda x: get_death_field(x, "Cause_English"))
```

## LICENSE

MIT License

Copyright (c) 2025
