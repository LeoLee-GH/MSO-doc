# Integration Overview

To integrate the ML Monitoring Service into your existing infrastructure, **no changes to your original model code or deployment are required**.

## Architecture Components
- **Edge Server** — Installed on-prem or in your VPC. Queries inference data streams for drift checks and analysis based on schedules or triggers.
- **Main Server** — Cloud-hosted. Integrates with ML workflows and/or model registry systems to access versioned models and support replication/testing.

## Network Communication
- **On-prem:** via Kubernetes internal networking.
- **Cloud:** within the same cloud security group or VPC subnet.

## What We Need (Least-Privilege)
- **Network:** Private access to the model inference endpoint(s) or batch job API within your VPC/cluster.
- **Service account:** One namespaced service identity scoped to the monitoring namespace only.
- **Model access:**
  - **Realtime:** Invoke/read-only to the model endpoint to collect non-identifying inference metadata and to run scheduled “golden-sample” checks.
  - **Batch:** Permission to trigger one designated job for golden-sample runs (no ability to list/modify other jobs).
- **Storage (your account):** Write-only to a dedicated bucket/prefix (metrics, drift scores, alerts). Optional read for your users/UI.
- **Registry (optional):** Read-only to model/version metadata to attribute metrics to versions.

## What We Collect/Store By Default
- **Yes:** Non-identifying metrics (drift statistics, performance/latency percentiles, alert states), anonymized request IDs, model/version tags.
- **No:** Raw inputs (e.g., images, DICOM), full outputs, or any PHI/PII. Storing samples for audit/QA is **opt-in** with de-identification controls you approve.

## Security & Data Policy (Summary)
- **Residency:** All data and logs stay in your region/account.
- **Encryption:** TLS in transit; at rest with your KMS/CMK.
- **RBAC/IAM:** Least-privilege roles; no permissions to modify models, pipelines, or datasets.
- **Auditability:** All actions logged via cloud/audit tooling; periodic access reviews supported.
- **Retention:** You set retention (e.g., 90 days for metrics, 1 year for audit logs).
- **Off-boarding:** You own the storage. Any vendor-side cache (if used for support) is purged on termination with deletion attestation.

## Integration Effort (No App/Code Changes)
- We deploy our container into your cluster/VPC (Helm/Terraform/YAML).
- You provide the scoped service account/roles and the metrics bucket path.
- We configure the agent to point at your model endpoint or batch job and set up golden-sample schedules and thresholds.

For more details, see [Data Handling Policies](../about/data-handling-policies.md).
