# Client Inquiry

**Q:** How is the monitoring tool integrated with existing ML models, what level of access/permissions are required, and what are the data storage policies?

**A:**  

## Integration Strategy
- No changes to your existing model code or deployment are required.
- Two main components: **Edge Server** (on-prem/VPC) and **Main Server** (cloud-hosted). Internal networking is used on-prem (Kubernetes), or within the same cloud security group/VPC subnet in cloud deployments.
- The Edge Server queries inference data streams and triggers scheduled drift checks; the Main Server integrates with workflow/orchestration tools to access the model registry and support replication/testing.
- We provide dashboards, optional data lakehouse access, and periodic monitoring reports for management, BI, and clinical stakeholders.

## Access / Permissions (Least-Privilege)
- **Network:** Private access to model inference endpoint(s) or batch job API.
- **Service Account:** One namespaced service identity scoped to the monitoring namespace only.
- **Model Access:**
  - **Realtime:** Invoke/read-only to collect non-identifying inference metadata and run golden-sample checks.
  - **Batch:** Trigger one designated job for golden-sample runs (no list/modify other jobs).
- **Storage (your account):** Write-only to a dedicated bucket/prefix for metrics, drift scores, and alerts; optional read for your users/UI.
- **Registry (optional):** Read-only to model/version metadata to attribute metrics to versions.

## Data Storage Policies
- **Default collection:** Non-identifying metrics, anonymized IDs, model/version tags.
- **No PHI/PII:** We do not store raw inputs or full outputs by default. Sample storage for audit/QA is opt-in with de-identification controls.
- See full details in [Data Handling Policies](../about/data-handling-policies.md).

## Notes for ML Teams
- Deployed entirely in your environment; all processing/storage remain under your control.
- Designed to communicate within your private network; no public endpoints required.
- We can adapt to different workflow tooling; in some pilots, we integrated with orchestration/registry systems with minimal API work.

If you have additional questions or would like to tailor the integration, please contact our support team listed in [Contact Information](../about/contact-information.md).
