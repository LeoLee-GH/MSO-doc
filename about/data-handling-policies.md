# Data Handling Policies

We deploy the monitoring service entirely in your environment (VPC/on-prem). All processing and storage remain under your control. By default, no Protected Health Information (PHI) leaves your network.

## Data We Collect and Store By Default
- **Yes:** Non-identifying metrics (drift statistics, performance/latency percentiles, alert states), anonymized request IDs, model/version tags.
- **No:** Raw inputs (images/DICOM), full outputs, or any PHI/PII. Storing samples for audit/QA is opt-in with de-identification controls you approve.

## Security & Data Policy
- **Residency:** All data and logs stay in your region/account.
- **Encryption:** TLS in transit; at rest with your KMS/CMK.
- **RBAC/IAM:** Least-privilege roles; no permissions to modify models, pipelines, or datasets.
- **Auditability:** All actions logged via CloudTrail/K8s audit; periodic access reviews supported.
- **Retention:** You set retention (e.g., 90 days for metrics, 1 year for audit logs).
- **Off-boarding:** You own the storage. If any vendor-side cache exists (e.g., support troubleshooting), we purge on termination and provide deletion attestation.

---
