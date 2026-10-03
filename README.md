# Dhamma Time

Dhamma Time is a peaceful Android meditation timer and Theravada Buddhist Dhamma listening app by **K2 Labs**.

- Android package: `com.k2labs.dhammatime`
- Developer: **K2 Labs**
- Contact: **k2lab.play@gmail.com**
- Privacy policy: [privacy/](./privacy/)
- Catalog documentation: [catalog/](./catalog/)

## Public repository purpose

This repository contains public resources used by Dhamma Time. It is intentionally separate from the private Android application source code.

It is intended to host:

- the Dhamma Time privacy policy;
- a small public landing page;
- Dhamma catalog metadata and version manifests;
- versioned catalog JSON files used by released versions of the Android app.

Audio files are **not** intended to be stored in this repository. Dhamma audio remains hosted by the relevant content providers or archival services.

## Catalog update design

When remote catalog updates are enabled, the Android app will keep a bundled catalog as an offline fallback and check a small remote manifest before downloading a newer catalog.

Planned public endpoint:

```text
/catalog/latest.json
```

Versioned catalogs will be immutable:

```text
/catalog/versions/catalog-v1.json
/catalog/versions/catalog-v2.json
...
```

The manifest will keep `catalogVersion` separate from `schemaVersion` so content updates do not unnecessarily change the JSON contract.

See [catalog/README.md](./catalog/README.md) for the catalog publishing rules.

## Privacy

Dhamma Time does not require an account, does not display third-party advertising, and the current Android project does not include advertising, analytics, or crash-reporting SDKs.

See the full [Privacy Policy](./privacy/).

## License and Dhamma content

This repository does not claim ownership of third-party Dhamma talks, recordings, biographies, or other materials referenced by the app. Rights in third-party content remain with their respective owners, publishers, monasteries, archives, or content providers.

---

© 2026 K2 Labs
