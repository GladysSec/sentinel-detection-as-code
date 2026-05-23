# Detection Lifecycle Management

This document describes how detection rules are managed from idea to retirement.

---

## Lifecycle Stages

1. **Identify Gap** – Use MITRE ATT&CK to find uncovered tactics/techniques.
2. **Write YAML** – Create a YAML file in `detections/` with KQL query, entity mapping, MITRE tags, and schedule.
3. **Test KQL** – Run the query in Sentinel Logs to verify syntax and results.
4. **Deploy** – Manually create the scheduled rule in Sentinel portal (copy KQL from YAML).
5. **Tune** – Observe alerts for false positives, adjust YAML and redeploy.
6. **Promote or Retire** – Keep active or archive after 6 months of low signal.

---

## Version Control Example

```bash
git add detections/AnomalousSignIn.yaml
git commit -m "Tune: exclude corporate IP ranges"
git tag v1.1.0
git push origin main --tags
