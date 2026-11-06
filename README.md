# Sovina resources

Public, static files the Sovina app reads. **No code, no secrets, nothing about any user.**

| File | What |
|---|---|
| `models/catalog.signed.json` | Signed list of on-device AI models Sovina may offer (fetched by the app at most once a day) |

## Why this is safe to host publicly

`catalog.signed.json` holds the model list (`payload`, base64 JSON) and an Ed25519 signature over it (`sig`). The app
only accepts it if the signature verifies against a public key built into the app, so editing this file - by anyone,
including whoever controls this repository - just makes phones ignore it. Every model listed is also pinned to a
SHA-256 that the download is checked against.

Public key (`keyId` `k20261106`, Ed25519, base64):

```
EWdxtc38fHF8l/1nYKeqxHWNQpJfe0twsVL8DF1NaUQ=
```

## Updating

Don't edit files here by hand. In the Sovina app repo: edit `catalog/catalog.json` (raise `sequence`), run
`node scripts/model-catalog.mjs sign catalog/catalog.json`, then copy `catalog/catalog.signed.json` to
`models/catalog.signed.json` here and push. Re-sign at least every 90 days (`expires_at`).
