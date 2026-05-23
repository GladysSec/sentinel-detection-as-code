# False Positive Tuning Guide

## Rule 1: Anomalous Sign‑in from Unusual Location

| Aspect | Details |
|--------|---------|
| Potential false positives | Business travel, VPN changes, new remote office |
| Tuning actions | Increase learning period to 14 days, exclude corporate IP ranges, suppress for 12 hours |

## Rule 2: Suspicious Role Assignment or Elevation

| Aspect | Details |
|--------|---------|
| Potential false positives | PIM activation, CI/CD pipelines (Terraform) |
| Tuning actions | Exclude service principals, monitor only outside business hours |

## Rule 3: Abnormal VM Creation or Deletion Burst

| Aspect | Details |
|--------|---------|
| Potential false positives | Auto-scaling groups, disaster recovery drills |
| Tuning actions | Ignore known resource groups, raise threshold to 10 per 5 minutes |
