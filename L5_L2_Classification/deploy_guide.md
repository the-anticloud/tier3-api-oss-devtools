# Deploy Guide — api-oss-devtools
**Platform:** Anticloud sovereign infrastructure | Air-gap capable
**Stack:** Python 3.11, Click, rich, pyinstrument, AIOSS_FORMAT

## Prerequisites
Python 3.11+, Click 8.1+, rich 13.7+, pyinstrument 4.6+

## AIOSS Integration
```bash
aioss init --module api-oss-devtools --output ./api_oss_devtools.aioss
aioss append --chain ./api_oss_devtools.aioss --payload ./output.bin --module api-oss-devtools
aioss verify --chain ./api_oss_devtools.aioss
```

## Air-Gap Deployment
```bash
# On networked machine:
pip download -r requirements.txt -d ./wheels/
# Transfer wheels/ to air-gap host, then:
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="api-oss-devtools",
    aioss_chain="./api_oss_devtools.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./api_oss_devtools.aioss --verbose
python -m api_oss_devtools.tests.smoke
```
