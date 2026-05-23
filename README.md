# sentinel-detection-as-code
Detection‑as‑Code for Microsoft Sentinel – YAML rules, KQL hunting, MITRE mapping
# Detection-as-Code for Microsoft Sentinel

[![Status](https://img.shields.io/badge/status-active-brightgreen)]()
[![Version](https://img.shields.io/badge/version-1.0.0-blue)]()
[![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT&CK-InitialAccess%2C%20PrivilegeEscalation%2C%20Impact-red)]()

This repository contains **detection rules** (YAML), **KQL hunting queries**, and **lifecycle documentation** for Microsoft Sentinel. The rules are deployed to a live Sentinel workspace and follow the Detection-as-Code (DaC) approach – version controlled, peer‑reviewable, and documented.

---

## 📁 Repository Structure

sentinel-detections/
├── detections/ # YAML detection rules (source of truth)
│ ├── AnomalousSignIn.yaml
│ ├── PrivilegeEscalation.yaml
│ └── SuspiciousActivityLog.yaml
├── queries/ # KQL hunting queries
│ └── hunting_anomalous_signin.kql
├── docs/
│ ├── false-positives-tuning.md
│ └── detection-lifecycle.md
└── README.md

---

## 🛡️ Detection Rules

| Rule Name | MITRE Tactics | Severity | Entity Mapping |
|-----------|---------------|----------|----------------|
| Anomalous Sign‑in from Unusual Location | InitialAccess, CredentialAccess | Medium | Account, IP, Location |
| Suspicious Role Assignment or Elevation | PrivilegeEscalation | High | Account, Azure Resource |
| Abnormal VM Creation or Deletion Burst | Impact, DefenseEvasion | Medium | Account, Azure Resource |

Each YAML rule includes:
- **KQL query** logic  
- **Query schedule & suppression**  
- **Entity mapping** for alert enrichment  
- **MITRE ATT&CK tactics & techniques** (T1078, T1098, T1485, etc.)  

---

## 📸 Live in Sentinel

> Screenshot of the three rules active in Microsoft Sentinel:

![active_rules](/images/active_rules.png)


---

## 🔍 Hunting Queries

The ![hunting_anomalous_signin.kql](/images/hunting_anomalous_signin.kql.png) folder contains proactive KQL hunts,

- **Impossible travel** – same user logging from two distant locations within a short time.

---

## 🧠 False‑Positive Tuning

See [`docs/false-positives-tuning.md`](./docs/false-positives-tuning.md) for:
- Common false‑positive scenarios
- Recommended tuning actions (exclusions, threshold changes, suppression)

---

## 🔄 Detection Lifecycle

The lifecycle document ([`docs/detection-lifecycle.md`](./docs/detection-lifecycle.md)) describes how rules are:

1. **Developed** (YAML in a branch)  
2. **Tested** (deployed to a dev Sentinel workspace)  
3. **Tuned** (based on observations)  
4. **Promoted** (merged to `main` and deployed to production)  
5. **Retired**  

---

## 🚀 Deployment

> **Note:** Because I am a tenant in a shared Azure environment, rules were **deployed manually** via the Sentinel portal to ensure full transparency and safety.  

For automated deployment (if you have appropriate permissions), use the PowerShell script in [`scripts/deploy-rules.ps1`](./scripts/deploy-rules.ps1).  
It reads each YAML and creates/updates the rule using `New-AzSentinelAlertRule`.

---

## 🧪 Testing

Test queries directly in Sentinel **Logs** blade before deploying a rule.  
Example – validate that `SigninLogs` contains data:

```kusto
SigninLogs | take 10 
```

## 🤝 Acknowledgements
Microsoft Sentinel documentation

MITRE ATT&CK framework

Azure Cloud Shell for safe experimentation

# Detection Lifecycle

## Stages
1. **Identify coverage gap** (e.g., missing MITRE technique)
2. **Write YAML** → commit to `feature/rule-name` branch
3. **PR & review** → merge to `main`
4. **Deploy to dev Sentinel** (script or manual)
5. **Observe & tune** → update YAML, commit, redeploy
6. **Promote to production**
7. **Archive after 6 months of low signal**

## Versioning example
```bash
git tag v1.0.0
git push origin v1.0.0
```

