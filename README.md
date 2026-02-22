# data-uk

Ammitto Data repository for UK (OFSI) Source.

This repository stores UK sanctions data in YAML format, fetched daily from the UK OFSI Sanctions List.

## Directory Structure

```
data-uk/
├── downloaded/           # Raw XML data
│   └── uk-sanctions.xml
├── processed/            # Individual YAML files
│   ├── _index.yaml      # Metadata
│   ├── afg0001.yaml     # Individual designations
│   ├── afg0002.yaml
│   └── ...              # ~5,700+ files
├── .github/workflows/
│   └── fetch.yml        # Daily fetch workflow
├── CLAUDE.md            # AI assistant guidance
└── README.md            # This file
```

## Data Source

- **Authority**: UK Office of Financial Sanctions Implementation (OFSI)
- **URL**: https://sanctionslist.fcdo.gov.uk/docs/UK-Sanctions-List.xml
- **Format**: XML (converted to YAML by ammitto CLI)

## Data Flow

1. **Fetch**: GitHub Actions runs `ammitto fetch uk --format yaml` daily at 6:00 AM UTC
2. **Process**: XML is parsed using Lutaml::Model source models
3. **Store**: Individual YAML files are saved to `processed/`
4. **Trigger**: On changes, triggers harmonization in `ammitto/data` repo

## YAML Schema

Each designation file contains:

```yaml
unique_id: AFG0001
names:
  names:
    - name6: "PRIMARY NAME"
      name_type: "Primary Name"
    - name6: "ALIAS"
      name_type: "Alias"
non_latin_names:
  names:
    - name_non_latin_script: "الاسم العربي"
regime_name: "The Afghanistan (Sanctions) (EU Exit) Regulations 2020"
individual_entity_ship: "Entity"  # or "Individual"
sanctions_imposed_indicators:
  asset_freeze: true
  travel_ban: false
  arms_embargo: false
  # ... more indicators
addresses:
  # ... address data
```

## Manual Update

```bash
# Install ammitto
gem install specific_install
gem specific_install https://github.com/ammitto/ammitto.git -b main

# Fetch latest data
ammitto fetch uk --format yaml --output-dir processed
```

## Related Repositories

- [ammitto/ammitto](https://github.com/ammitto/ammitto) - Core gem
- [ammitto/data](https://github.com/ammitto/data) - Central harmonized data
- [ammitto/data-eu](https://github.com/ammitto/data-eu) - EU data
- [ammitto/data-un](https://github.com/ammitto/data-un) - UN data
- [ammitto/data-us](https://github.com/ammitto/data-us) - US data
- [ammitto/data-wb](https://github.com/ammitto/data-wb) - World Bank data
