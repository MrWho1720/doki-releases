# Lumera Client — releases

Public installers, checksums and updater metadata for **Lumera Client**, a
Minecraft launcher and in-game client built on Rust and Tauri 2.

There is no source code here. This repository exists so that the binaries can be
public while the source stays private, and so that the in-app updater has
something to read without credentials.

## What is in a release

Every published release carries the complete set for both shipped platforms —
never a partial matrix, because the workflow that builds it stages everything
privately and only publishes once all of it has been validated.

| File | What it is |
| --- | --- |
| `LumeraClient_<version>_windows-x86_64.msi` | Windows installer |
| `LumeraClient_<version>_linux-x86_64.AppImage` | Linux, self-contained, self-updating |
| `LumeraClient_<version>_linux-x86_64.deb` | Debian / Ubuntu package |
| `LumeraClient_<version>_linux-x86_64.rpm` | Fedora / RHEL package |
| `*.sig` | Detached updater signatures |
| `SHA256SUMS.txt` | Checksums for everything above |
| `latest.json` | Metadata the in-app updater reads |

**There is no macOS build.** Lumera holds no Apple Developer signing identity,
and an un-notarized DMG is refused by Gatekeeper on any Mac whose owner has not
been talked through overriding it. Rather than publish a file most people cannot
open, macOS is left out of the matrix until the certificate is real.

## Which file to take

- **Windows** — the `.msi`. It is the only Windows asset, so there is nothing to
  get wrong.
- **Linux** — the `.AppImage` if you want the app to keep itself updated, or the
  `.deb` / `.rpm` if you would rather your package manager owned it.

## Verifying a download

```sh
sha256sum -c SHA256SUMS.txt --ignore-missing
```

On Windows:

```powershell
Get-FileHash .\LumeraClient_1.0.0_windows-x86_64.msi -Algorithm SHA256
```

Compare the result against the matching line in `SHA256SUMS.txt`.

## About the warnings you will see

Windows installers are **not Authenticode signed** yet, so SmartScreen shows an
Unknown Publisher warning on first install. Choose **More info → Run anyway**.
That warning is the honest state of the build, not a fault on your machine, and
it stays until Lumera holds a certificate.

That is a separate mechanism from update integrity. Every automatic update
payload is signed with the Lumera updater key and verified before it is applied,
so an update cannot be substituted even though the first install is unsigned.

## Anything not published here is not an official build

If a Lumera installer did not come from this repository's releases page, it did
not come from Lumera. Local and development builds are not releases.
