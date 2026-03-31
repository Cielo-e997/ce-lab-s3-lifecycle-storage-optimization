# Lab M7.07 - S3 Lifecycle Policies and Storage Optimization

## Overview

In this lab, I implemented a storage optimization strategy using Amazon S3 lifecycle policies and storage class transitions. The goal was to reduce storage costs by automatically moving data to cheaper tiers based on access patterns, while maintaining usability and compliance.

---

## What I Built

- Three S3 buckets representing different data tiers:
  - Logs (frequently accessed, short-lived)
  - Analytics data (unpredictable access)
  - Archive (rarely accessed, long-term storage)
- Uploaded sample objects to simulate real-world workloads
- Performed a storage usage analysis using AWS CLI
- Applied lifecycle policies to automate storage transitions
- Configured Intelligent-Tiering for dynamic cost optimization
- Enabled S3 Storage Lens for account-wide visibility
- Created a cost savings report based on before/after comparison

---

## Storage Architecture

### Logs Bucket
Designed for high-ingestion, time-based data with predictable decay in access.

Lifecycle policy:
- Days 0–29 → S3 Standard
- Days 30–89 → S3 Standard-IA
- Days 90–364 → S3 Glacier Flexible Retrieval
- Day 365 → Object expiration (deletion)

Additional rule:
- Abort incomplete multipart uploads after 7 days

---

### Data Bucket (Analytics)

Designed for datasets with unpredictable access patterns.

Lifecycle policy:
- Day 30 → Transition to S3 Intelligent-Tiering

Intelligent-Tiering configuration:
- Day 90 → Archive Access
- Day 180 → Deep Archive Access

This removes the need to manually predict access patterns.

---

### Archive Bucket

Used to simulate long-term storage of compliance data.  
(No lifecycle policy applied in this lab, but would typically go directly to Glacier or Deep Archive in real scenarios.)

---

## Storage Analysis (Baseline)

| Bucket  | Objects | Size       | Storage Class |
|--------|--------|------------|---------------|
| logs   | 20     | ~6.29 MB   | STANDARD      |
| data   | 5      | ~9.85 MB   | STANDARD      |
| archive| 3      | ~12.36 MB  | STANDARD      |

All objects were initially stored in S3 Standard, which is the most expensive storage class.

---

## Cost Optimization Strategy

The optimization approach is based on:

- Moving aging data to cheaper storage classes automatically
- Using Intelligent-Tiering when access patterns are unknown
- Cleaning up unused or incomplete uploads
- Maintaining performance for recent data while minimizing long-term cost

---

## Projected Cost Savings

Because this lab uses small sample data, the absolute cost savings are minimal. However, the percentage reduction reflects real-world impact.

### Before Optimization
- All data stored in S3 Standard

### After Optimization
- Logs → IA → Glacier → Expiration
- Data → Intelligent-Tiering + Archive tiers
- Archive → suitable for Deep Archive in production

### Estimated Savings
- **~40%–70% monthly reduction**
- Savings scale significantly with larger datasets (GB → TB)

---

## Storage Lens

S3 Storage Lens was enabled to provide:
- Account-level visibility into storage usage
- Activity metrics for monitoring access patterns
- Prefix-level insights for deeper analysis

Note: Metrics may take 24–48 hours to populate.

---

## Validation Performed

- Verified bucket creation and tagging
- Confirmed object uploads across all buckets
- Validated lifecycle configuration for logs bucket
- Verified Intelligent-Tiering configuration for data bucket
- Confirmed Storage Lens configuration is active

---

## Screenshots

- s3-buckets-list.png
- s3-logs-objects.png
- s3-storage-summary.png
- logs-lifecycle-policy.png
- data-intelligent-tiering.png
- storage-lens-config.png

---

## Key Takeaways

- Lifecycle policies automate cost optimization with zero ongoing effort
- Intelligent-Tiering is ideal for unpredictable workloads
- Glacier tiers provide massive savings for cold data
- Storage Lens helps identify optimization opportunities across accounts
- Small architectural decisions in storage can lead to large cost reductions at scale