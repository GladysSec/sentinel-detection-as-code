# False Positive Tuning Guide

This document helps identify and reduce false positives for the three detection rules deployed manually in this project.

---

## Rule 1: Anomalous Sign‑in from Unusual Location

| Aspect | Details |
|--------|---------|
| **Potential false positives** | – Business travel to new cities<br> – VPN exit points changing (home vs office)<br> – New remote office opening |
| **How to identify** | Check if the user has a history of occasional travel or if the location is a known company office. |
| **Tuning actions** | - Increase learning period to 14 days<br> - Exclude corporate IP ranges<br> - Suppress repeated alerts for 12 hours |
| **KQL modification example** | ```kusto
// Add after the join
| where IPAddress !in ("203.0.113.0/24")
``` |

---

## Rule 2: Suspicious Role Assignment or Elevation

| Aspect | Details |
|--------|---------|
| **Potential false positives** | – Planned PIM activations<br> – Terraform / CI/CD pipelines assigning roles<br> – IT admins reviewing access |
| **How to identify** | Look at the `Caller` field – service principals or known admin accounts often cause FPs. |
| **Tuning actions** | - Exclude service principals: `where Caller !in ("sp-terraform")`<br> - Monitor only outside business hours<br> - Require additional approval for Global Admin role |
| **KQL modification example** | ```kusto
| where Caller !contains "sp-"
| where Caller !contains "azure-pipelines"
``` |

---

## Rule 3: Abnormal VM Creation or Deletion Burst

| Aspect | Details |
|--------|---------|
| **Potential false positives** | – Auto‑scaling groups (VM scale sets)<br> – Disaster recovery drills<br> – Legitimate batch VM creation (labs, training) |
| **How to identify** | Check the `ResourceGroup` name for keywords like `autoscale`, `dr`, `test`, `lab`. |
| **Tuning actions** | - Ignore known resource groups<br> - Raise threshold from 5 to 10 creations per 5 minutes<br> - Add time filter (e.g., ignore 2–4 AM if drills occur then) |
| **KQL modification example** | ```kusto
| where ResourceGroup !in ("rg-autoscale", "rg-dr")
``` |

---

## General tuning best practices

- Start with lower severity and increase after tuning.
- Document every change in this file and update the YAML in GitHub.
- Review false positives monthly.

**Last updated:** May 2026
