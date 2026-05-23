# Detection Lifecycle Management

This document describes how detection rules are managed from idea to retirement in this project.

---

## Lifecycle Stages

```mermaid
graph LR
    A[1. Identify Gap] --> B[2. Write YAML]
    B --> C[3. Test KQL in Logs]
    C --> D[4. Manual Deploy to Sentinel]
    D --> E[5. Tune & Document]
    E --> F[6. Promote / Retire]
