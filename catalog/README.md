# Dhamma Time Catalog

This directory is reserved for public Dhamma catalog metadata consumed by the Dhamma Time Android app.

## Intended structure

```text
catalog/
├── README.md
├── latest.json
└── versions/
    ├── README.md
    ├── catalog-v1.json
    ├── catalog-v2.json
    └── ...
```

## Important rule

**Do not publish `latest.json` until remote catalog support has been implemented and tested in the Android app.**

The app must always include a bundled catalog as an offline and recovery fallback.

## Manifest contract

When activated, `latest.json` should use a small manifest similar to:

```json
{
  "catalogVersion": 1,
  "schemaVersion": 1,
  "updatedAt": "2026-10-03T00:00:00Z",
  "catalogUrl": "versions/catalog-v1.json",
  "sha256": "<sha256-of-catalog-file>",
  "minAppVersionCode": 3
}
```

### Fields

- `catalogVersion`: increments whenever public catalog content changes.
- `schemaVersion`: increments only when the JSON structure changes incompatibly.
- `updatedAt`: UTC ISO-8601 publication timestamp.
- `catalogUrl`: relative URL of the immutable catalog file.
- `sha256`: SHA-256 checksum of the exact published catalog bytes.
- `minAppVersionCode`: lowest Android app version code allowed to consume the catalog.

## Android update flow

1. Load the valid locally cached catalog, otherwise the bundled catalog.
2. Render the UI without waiting for the network.
3. Check `latest.json` in the background.
4. Compare the remote `catalogVersion` with the installed local version.
5. Reject unsupported `schemaVersion` or `minAppVersionCode`.
6. Download the referenced immutable catalog file.
7. Verify SHA-256 before parsing.
8. Validate required catalog fields and identifiers.
9. Write to a temporary file/database transaction.
10. Atomically promote the validated catalog and persist the new version.
11. Keep the previous valid catalog available for recovery if activation fails.

## Publishing rules

- Never overwrite an already published `catalog-vN.json`.
- Every content change creates a new version file.
- Update `latest.json` only after the new version has passed validation.
- Keep stable identifiers for Sayadaws, collections, and talks.
- Do not store MP3 binaries in this repository.
- Prefer HTTPS audio and artwork URLs.
- Do not include secrets, tokens, private URLs, credentials, or personal data.
- Validate duplicate IDs and malformed URLs before publishing.
- Update the manifest last so clients never discover a partially published catalog.

## Rollback

If a newly published catalog is faulty, point `latest.json` back to the last known-good version. Clients should accept rollback only according to the Android app's explicit catalog policy; production code should not silently downgrade without a defined recovery rule.
