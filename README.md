# Bulk Property Importer

A Python utility for extracting projects and unit inventories from property price-list PDFs, then creating those records through a configurable REST API.

## What it does

- Extracts tables from a property price-list PDF with `pdfplumber`.
- Normalizes project and unit data including unit code, bedrooms, bathrooms, areas, price, pool type, and availability.
- Avoids duplicate project creation by checking the target API first.
- Creates projects and units for an allowlisted set of project names.

## Requirements

- Python 3
- A compatible PDF price list
- API access for the destination property platform

Install the Python dependencies:

```bash
python -m pip install requests pdfplumber
```

## Configuration

Copy the template, then add values for your own API environment:

```bash
cp .env.example .env
```

The script reads its configuration from environment variables. It does not load `.env` files automatically, so export the variables in your shell or use your preferred local environment loader.

| Variable | Purpose |
| --- | --- |
| `PROPERTPRO_API_BASE_URL` | Base URL for the destination API |
| `PROPERTPRO_ORGANIZATION_ID` | Destination organization ID |
| `PROPERTPRO_ACCESS_TOKEN` | Bearer token for the destination API |
| `PRICE_LIST_PDF` | Optional path to the source PDF, defaulting to `FullPricelist_B-1.pdf` |

Example for a Bash-compatible shell:

```bash
export PROPERTPRO_API_BASE_URL="https://your-api.example/api/dev/v1"
export PROPERTPRO_ORGANIZATION_ID="123"
export PROPERTPRO_ACCESS_TOKEN="your-token"
export PRICE_LIST_PDF="FullPricelist_B-1.pdf"
```

## Run

```bash
python import_project.py
```

The importer can create remote records. Review the target API, organization ID, allowlisted project names, and source PDF before running it against a live environment.

## Diagnostics

`test_tables.py` prints the tables extracted from the source PDF and is useful for checking layout changes before an import.

```bash
python test_tables.py
```

## Limitations

This is a project-specific import tool, not a general PDF ingestion framework. The expected PDF layout and default property fields are tailored to the source data it was built for.
