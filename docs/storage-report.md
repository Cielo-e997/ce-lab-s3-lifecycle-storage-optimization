# S3 Storage Optimization Report

## Current State

| Bucket  | Objects | Size | Storage Class | Monthly Cost |
|---------|---------|------|---------------|--------------|
| logs    | 20      | 6.29 MB | STANDARD | ~$0.00014 |
| data    | 5       | 9.85 MB | STANDARD | ~$0.00022 |
| archive | 3       | 12.36 MB | STANDARD | ~$0.00028 |

## Lifecycle Policies Applied

### Logs Bucket
- Days 0-29: S3 Standard
- Days 30-89: S3 Standard-IA
- Days 90-364: S3 Glacier Flexible Retrieval
- Day 365: Expire objects
- Incomplete multipart uploads: Abort after 7 days

### Data Bucket
- Days 0-29: S3 Standard
- Day 30+: S3 Intelligent-Tiering
- Day 90+: Archive Access tier
- Day 180+: Deep Archive Access tier

## Storage Lens
- Account-level Storage Lens configuration enabled
- Activity metrics enabled
- Prefix-level storage metrics enabled

## Projected Savings

Because the lab uses small sample files, the direct dollar savings are minimal in absolute terms. However, the lifecycle design reflects the same optimization approach used at production scale.

- Estimated monthly savings: **~40-70% reduction** depending on object age and access patterns
- Estimated annual savings: small in this lab dataset, but substantial when scaled to real multi-GB or TB workloads

## Summary

This lab demonstrated how to:
- audit current S3 storage usage
- apply lifecycle transitions for logs
- configure Intelligent-Tiering for analytics data
- enable S3 Storage Lens for account-wide monitoring
- estimate savings from moving aging data out of S3 Standard