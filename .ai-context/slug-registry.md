# Slug Registry

## Purpose

This is the single authoritative record of all feature slugs — active, retired,
archived, and renamed — for this project.

Every slug must be registered here when first assigned. This registry is the
primary collision-detection mechanism: before assigning a new slug, search this
file to confirm the slug is not in use and has not been retired.

Governing rule: `.agent/rules/int-sdd-naming.md` §5.4

---

## How to Register a New Slug

1. Confirm the proposed slug is not already in the registry (any status).
2. Confirm the slug format is valid (kebab-case, 3–5 words, no prohibited patterns).
3. Run the naming check: `.agent/workflows/naming-identifier-check.md`.
4. Add a row to the Active Slugs table.
5. Record the date and linked spec path.

## Slug Status Definitions

| Status | Meaning |
| ------ | ------- |
| `Active` | Feature spec exists and is in an active lifecycle state |
| `Retired` | Feature was deprecated or superseded; slug permanently unavailable |
| `Archived` | Feature was archived; slug permanently unavailable |
| `Renamed` | Slug was renamed through the controlled process; old slug permanently unavailable |

---

## Active Slugs

| Slug | Status | Assigned date | Spec path | Notes |
| ---- | ------ | ------------- | --------- | ----- |

_No feature slugs have been assigned. This registry is empty until the first real
feature specification is created._

---

## Retired / Archived / Renamed Slugs

| Old slug | Final status | Date retired | Replaced by | Notes |
| -------- | ------------ | ------------ | ----------- | ----- |

_No slugs have been retired. This table will be populated when features are
deprecated, superseded, or archived._

---

## Validation Notes

A slug appearing in either table is permanently unavailable for reuse.
A proposed slug that does not appear in either table may be used (subject to
semantic near-match review per `.agent/rules/int-sdd-naming.md` §5.2).
