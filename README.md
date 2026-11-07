# insurance_open_data

Scripts and pipelines that collect insurance and economic data from open sources and turn it into clean, analysis-ready datasets. Maintained by [Open-Source Modelling](https://github.com/open-source-modelling).

Each project lives in its own repository and is included here as a [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules). This repository is the index: every folder below links to a specific version of a project repository.

## Projects

| Folder | Source | What you get |
|---|---|---|
| [`EIOPA_all_curves`](https://github.com/open-source-modelling/EIOPA_all_curves) | EIOPA risk-free rate (RFR) releases | Monthly history of Solvency II risk-free yield curves for every published country |
| [`IMF_GDP_pipeline`](https://github.com/open-source-modelling/IMF_GDP_pipeline) | World Bank indicator `NY.GDP.MKTP.PP.CD` | Validated GDP, PPP (current international $) by country and aggregate |
| [`SFCR_using_Mistral_2024`](https://github.com/open-source-modelling/SFCR_using_Mistral_2024) | Insurers' Solvency and Financial Condition Reports (SFCR) | Balance sheet tables from Italian life insurers, transcribed with Mistral AI |
| [`SFCR_using_Mistral_2025`](https://github.com/open-source-modelling/SFCR_using_Mistral_2025) | Insurers' SFCRs | Balance sheet, own funds and SCR tables from Italian life insurers, with cross-validation |

### EIOPA risk-free rate curves

Python pipeline that extracts EIOPA's monthly RFR Excel workbooks into tidy CSV history, so you can look up or rebuild a yield curve for any covered date and country without opening the original spreadsheets.

- **Data:** Smith–Wilson calibration vectors (Qb), curve parameters (UFR, alpha, LLP, CRA, VA) and official spot rates, monthly from December 2014, with and without volatility adjustment.
- **Main files:** `data/yield_curves.csv`, `data/curve_parameters.csv`, `data/qb_vectors.csv`.
- **Rebuilding curves:** the project README includes Python code that reconstructs the full spot curve with the Smith–Wilson method for any month in the history.

### GDP (PPP)

Automated download, validation and publication of the World Bank indicator **GDP, PPP (current international $)**, built for periodic pension-fund reporting.

- **Data:** one row per country or aggregate, one column per year from 1960.
- **Fail-safe publishing:** every run goes through a set of integrity checks. If a blocking check fails, nothing is published and the previous file is left untouched. Each run is recorded in `manifest.json` with a validation report.
- **Main file:** `data/processed/gdp_ppp_wide.csv`.

### SFCR tables with Mistral AI

Turns tables in insurers' SFCR PDF filings into structured data using Mistral AI, then checks the numbers for internal consistency.

- **2024 edition:** balance sheet table for the 18 main Italian life insurers, in Jupyter notebooks covering extraction, processing and cross-validation.
- **2025 edition:** balance sheet (S.02.01.02), own funds (S.23.01.01) and SCR under the standard formula (S.25.01.21) for 22 Italian life insurers. Refactored into Python scripts using Mistral OCR and Pydantic schemas, with cross-checks and a measure of OCR accuracy.
- Both editions need a [Mistral API key](https://docs.mistral.ai/getting-started/quickstart/).

## Getting the code

Clone this repository together with all the projects:

```bash
git clone --recurse-submodules https://github.com/open-source-modelling/insurance_open_data.git
```

If you already cloned it without `--recurse-submodules`, the project folders will be empty. Fill them with:

```bash
git submodule update --init --recursive
```

To bring every project up to its latest version:

```bash
git submodule update --remote
```

`EIOPA_all_curves` carries the full data history and is large, so the first clone can take a while. If you only need one project, clone its repository directly.

Each project has its own README with setup instructions, requirements and a description of its data.

## Related

- [insurance_python](https://github.com/open-source-modelling/insurance_python): actuarial algorithms in Python, including the Smith–Wilson implementation used to rebuild the EIOPA curves.

## Contact

These pipelines feed our own actuarial models, and feedback is one of the main reasons we publish them. Open an issue in the relevant project, or write to gregor@osmodelling.com.

## License

MIT. See [`LICENSE`](LICENSE). Each project is also MIT-licensed and carries its own license file.
