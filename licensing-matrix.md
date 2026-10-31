# Euroton; Licensing & Trademark Matrix

Phase 0 deliverable. Source repo audited: a checkout of ProtonMail/WebClients (commit `23b362f907`, captured 2026-05-31).

Going forward, downstream consumers in this workspace read from [`proton-webclients-mirror/`](./proton-webclients-mirror/), a single-commit-sync mirror of upstream. This audit will need a re-run on every sync that touches license-bearing files (`LICENSE`, any `package.json`, any `applications/*/src/assets/`).

## 1. Headline verdict

- **Root license: GPL-3.0.** Fork is legally permitted. No CLA file found.
- **No bundled proprietary code, no submodules, no `LICENSES/` aggregator**; fork has no hidden license obligations beyond GPL-3.0 (and the 7 permissive sub-packages noted below, which are *more* permissive, not less).
- **Trademark scope is significant**; ~3,500+ "Proton" references across constants, assets, strings, locales. De-branding is a real effort but mechanical.
- **No CLA** = no copyright concentration upstream. Forking does not require permission.

## 2. License table

### 2.1 Repository root

| Item | Value |
|---|---|
| Root `LICENSE` | `GNU GENERAL PUBLIC LICENSE Version 3, 29 June 2007` |
| Root `package.json` license field | `"GPL-3.0"` |
| `CONTRIBUTING.md` | absent |
| `CLA.md` | absent |
| `NOTICE` | absent |
| `.gitmodules` | absent |
| Vendored third-party trees (`vendor/`, `third_party/`) | absent (one TS file under `applications/pass-extension/vendor/`, not a vendored library) |

### 2.2 `packages/*`; non-GPL outliers

These 7 packages use more-permissive licenses. **All are GPL-compatible**; using them downstream in a GPL-3.0 or AGPL-3.0 work is fine.

| Package | License | Notes |
|---|---|---|
| `packages/encrypted-search` | MIT | LICENSE file present |
| `packages/i18n` | ISC | LICENSE file present |
| `packages/key-transparency` | MIT | LICENSE file present. ⚠️ Security-sensitive. Dependencies include `@protontech/crypto` (external scope; verify license separately at install time). |
| `packages/pack` | MIT |; |
| `packages/react-polymorphic-types` | MIT | LICENSE file present |
| `packages/styles` | ISC | LICENSE file present |
| `packages/stylelint-config-proton` | ISC | LICENSE.md present |

### 2.3 `packages/*`; no `license` field in package.json (21 packages)

These inherit from the workspace (GPL-3.0). No risk, but flagged for completeness in case any are extracted into Euroton standalone; they should get explicit `license` fields on extraction.

```
ai-assistant, chargebee, codeshift, colors, cross-storage, docs-core,
docs-proto, docs-shared, drive, drive-store, eslint-plugin-custom-rules,
features, global-types, hooks, icons, jest-env, llm, metrics, nav,
polyfill, raw-images, recovery-kit, redux-shared-store,
redux-shared-store-types, redux-utilities, sanitize, types, unleash,
utils
```

### 2.4 `applications/*`

All 16 GPL-3.0-licensed applications use the standard license. 4 lack explicit `license` field (`authenticator`, `docs-editor`, `pdf-ui`, `preview-sandbox`); inherit GPL-3.0 from workspace.

### 2.5 Specific risk-flagged packages

| Package | Status |
|---|---|
| `packages/crypto/` | **Does not exist**; crypto is via `@protontech/crypto` (external dependency, audit at install) |
| `packages/srp/` | **Does not exist**; SRP lives in `packages/shared/lib/srp.ts` (GPL-3.0) |
| `packages/key-transparency/` | MIT, but depends on `@protontech/crypto` |
| `packages/pass/` | GPL-3.0, no proprietary deps |
| `packages/wallet/` | GPL-3.0, no proprietary deps |

**Action for Euroton-server (Phase 2):** when adding `@protontech/crypto` as a dependency, capture its license at the package-lock level and add to a `THIRD_PARTY_NOTICES` file. Likely OpenPGP.js fork; license should be permissive.

### 2.6 Reference clones (not on the dependency line)

These live at the workspace root, are gitignored, and are read-only references; not built, not vendored, not redistributed.

| Path | Upstream | License | Notes |
|---|---|---|---|
| `proton-bridge/` | github.com/ProtonMail/proton-bridge | GPL-3.0 | Reference for `euroton-imap-bridge` when unparked. |
| `go-proton-api/` | github.com/ProtonMail/go-proton-api | MIT (2020 J. Houlahan, 2022 Proton AG) | Reference for wire-format Go structs (`*_types.go`) and server route handlers (`server/`). Cross-checks `openapi-spec/` extractor output. |

If any handler shape or struct is ever ported into `euroton-server/` from `go-proton-api/`, capture the attribution in `THIRD_PARTY_NOTICES` (MIT requires it). Mere shape-conformance from reading the code is not derivative work; verbatim copy is.

## 3. Trademark inventory

### 3.1 Surface area

| Surface | Volume |
|---|---|
| `"Proton Mail" / "ProtonMail"` occurrences | 152 |
| `"proton.me"` occurrences | 1,297 |
| `"protonmail.com"` occurrences | 171 |
| Subdomain references (`account.proton.*`, `api.proton.*`, ...) | 496 |
| Other product-name combinations (Calendar, Drive, Pass, VPN, Account) | ~384 |
| **Total brand surface (estimate)** | **~3,500 references** |

### 3.2 Canonical brand source-of-truth

`packages/shared/lib/constants.ts`:

```typescript
export const BRAND_NAME = 'Proton';
export const MAIL_APP_NAME = `${BRAND_NAME} ${MAIL_SHORT_APP_NAME}`;
export const VPN_HOSTNAME = 'account.protonvpn.com';
export const PROTON_DOMAINS = [
  'protonmail.com', 'protonmail.ch', 'protonvpn.com',
  'protonstatus.com', 'gdpr.eu', 'protonvpn.net', 'pm.me',
  'protonmailrmez3lotccipshtkleegetolb73fuirgj7r4o4vfu7ozyd.onion',
  'proton.me',
];
export const APPS = {
  PROTONACCOUNT: 'proton-account',
  PROTONMAIL:    'proton-mail',
  // ...18+ entries
};
```

**Strategy:** Replace `BRAND_NAME` at the constants level wherever possible; it propagates to ~70% of surface strings automatically via template literals. The remaining 30% are hardcoded URLs, asset paths, and i18n catalogs (manual sweep needed).

### 3.3 Logo and icon assets (de-brand targets)

- `packages/styles/assets/img/illustrations/proton-logo.svg`
- `packages/styles/assets/img/illustrations/proton-wallet-logo.svg`
- `packages/styles/assets/img/illustrations/mail-plus-logo.svg`
- `packages/pass/assets/protonpass-account.svg`
- `applications/pass-extension/public/assets/protonpass-logo.svg`
- `packages/styles/assets/img/drive/big-logo-white.svg`
- `packages/icons/icons/IcBrandProton*.tsx` (10 React components)
- 14 `favicon.config.js` files across `applications/*`

Per-app asset counts: Mail 102, Calendar 33, Drive/Pass/Account 0 (these pull from shared `packages/icons/` and `packages/styles/assets/`).

### 3.4 i18n posture

Translation catalogs are baked into source under `applications/*/locales/*.json` (e.g., `applications/mail/locales/` has 34 files). De-branding requires per-locale string replacement. Sample: `fr_FR.json` contains 10 "Proton" string occurrences in the Mail app alone.

### 3.5 Brand color tokens

Colors are **semantic** (`--primary`, `--danger`, etc.) generated dynamically by SCSS at `packages/styles/scss/utilities/_colors.scss`; no hardcoded "Proton Blue" hex. Re-skinning Euroton requires only redefining CSS variable values, not invasive code changes. **This is a meaningful win; branding is cleanly separated.**

## 4. Per-phase go/no-go

| Phase | License clearance | Trademark blocker | Verdict |
|---|---|---|---|
| **Phase 1**; `openapi-spec/` | GPL-3.0 inherits cleanly. Spec is generated *about* the upstream code, not derived from it byte-for-byte. | None; spec contains no Proton branding by construction (only endpoint shapes). | ✅ **Go** |
| **Phase 2**; `euroton-server/` | AGPL-3.0 compatible with GPL-3.0 upstream client. `@protontech/crypto` license must be confirmed at install. | Server has no UI; no trademark exposure. | ✅ **Go** |
| **Phase 3**; `euroton-selfhost/` | GPL-3.0 (client forks inherit). Must publish all client source per GPL. Must ship `NOTICE` crediting Proton. | **Mandatory full de-brand sweep** before public release. CI gate on trademark grep. | ✅ **Go with hard gate on de-brand** |
| **Phase 4**; `euroton-imap-bridge/` | AGPL-3.0. Same as Phase 2. | Same as Phase 2 (server-only). | ✅ **Go** |
| **Phase 2 companion**; `euroton-godmode/` | AGPL-3.0. Operator-facing UI that talks to `euroton-server`'s admin RPCs; inherits the same network-service licensing tier so a hosted fork must publish modifications. | None in the UI itself; operator-only deployment; the operator chooses whether to skin or rebrand. | ✅ **Go** |

## 5. CI gates (to implement during Phases 1–3)

- **License gate**: `license-checker` in CI on every package.json, fail on any non-GPL-compatible license added downstream.
- **Trademark gate** (Phase 3 distribution only): `grep -r -i 'proton'` against built artifacts (excluding `NOTICE` and license attribution files), fail on any match.
- **GPL compliance gate**: every release artifact must ship its corresponding source archive or link.

## 6. Open items (defer, but track)

1. Confirm `@protontech/crypto` license at npm install time. Add to `THIRD_PARTY_NOTICES`.
2. If any pulled npm dependency carries a "no-derivative" clause, route around it before Phase 2 ships.
3. Trademark research: file a quick USPTO/EUIPO check on "Proton Mail" / "Proton" to confirm what's registered vs. claimed (the Mail and VPN marks are almost certainly registered). Euroton must be visually + linguistically distinct (verify).
4. Decide AGPL-3.0 vs. GPL-3.0 for `euroton-server` after consulting with whoever will operate it; AGPL forces hosted-fork source disclosure, which may deter commercial reuse. Plan currently defaults to AGPL-3.0; flip if this is undesirable.
