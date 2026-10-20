# nglihong-git.github.io — NG Hub

Public key hosting and OAuth callback site for **NG Hub**, a personal,
single-user [Tesla Fleet API](https://developer.tesla.com) application.

- **Owner / user:** the repository owner only. The application is used with
  the owner's own Tesla vehicle and Tesla account.
- **Commercial use:** none. This is a personal, non-commercial project.
  No third-party users, no data sharing, resale, or advertising.

## Why this repository exists

Tesla requires every Fleet API application to publish a public key on a
domain the developer controls, at a fixed path, so Tesla can verify the
application before it may send vehicle commands. This site is that domain:

| Path | Purpose |
| --- | --- |
| `/.well-known/appspecific/com.tesla.3p.public-key.pem` | The application's EC public key (P-256), served verbatim for Tesla's verification. |
| `/callback.html` | OAuth redirect target for the Tesla authorization flow; displays the returned authorization code so the owner can complete sign-in. |
| `/index.html` | Landing page identifying the site. |
| `/.nojekyll` | Empty marker so GitHub Pages serves the `.well-known` folder instead of ignoring dot-folders (Jekyll default). |

## Security notes

- Only the **public** key is published here — that is its purpose. The
  matching private key is **not** in this repository, has never been
  committed, and is stored locally by the owner with restricted file
  permissions. Command signing happens locally, never from this site.
- This site is fully static: it collects, stores, and transmits no data.
  The callback page reads the authorization code from the URL in the
  visitor's own browser only.
- If the key pair is ever rotated, the PEM file above is replaced and the
  change is recorded in the changelog below with its verification result.

## Verification

To confirm the published key is live and intact:

```sh
curl -s https://nglihong-git.github.io/.well-known/appspecific/com.tesla.3p.public-key.pem
```

Expected: HTTP 200 and a PEM block beginning `-----BEGIN PUBLIC KEY-----`,
identical to the public key registered with the NG Hub application.

## Changelog

- **2026-10-20** — Repository created and site published. Public key
  generated (EC prime256v1 / P-256), committed at the required
  `.well-known` path, and verified live: HTTP 200, served PEM byte-identical
  to the owner's local public key. Homepage and callback page verified
  HTTP 200. Origin and Redirect values for the Tesla developer application
  "NG Hub" designated as `https://nglihong-git.github.io` and
  `https://nglihong-git.github.io/callback.html` (form entry in progress).

## Related documentation

The full build log for the NG Hub application — application registration
drafts with verified character counts, key generation details, and
step-by-step verification results — is maintained by the owner alongside
the private half of this setup and is not published, by design.
