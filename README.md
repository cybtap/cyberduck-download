# Cyberduck 9.6.0.46406 Download for Windows — S3 and SFTP

Cyberduck is a file browser for remote storage. It speaks S3 and S3-compatible endpoints, OpenStack Swift, WebDAV, SFTP, FTP and the consumer clouds, and shows them all in the same Finder-like window. Uploads can be encrypted client-side with Cryptomator, versions and ACLs are editable from the inspector, and large transfers resume instead of restarting.

Everything in the program hangs off a bookmark: a saved endpoint, credentials, protocol and optional path. There is no site manager separate from the browser window, and switching between an S3 bucket and an SFTP host is a double-click rather than a reconnect dialog.

<p align="center">
  <a href="https://cyberduck.io/download/">
    <img src="https://img.shields.io/badge/DOWNLOAD-Cyberduck_9.6.0.46406-1B5E9B?style=for-the-badge&logo=windows&logoColor=white" alt="Download Cyberduck">
  </a>
</p>

| | |
|---|---|
| **Version** | 9.6.0.46406 |
| **Package** | installer from the download page |
| **Architecture** | x64 |
| **Platform** | Windows 10, Windows 11 |
| **License** | GPLv3 |

## Download

| Package | Link |
|---|---|
| **Cyberduck 9.6.0.46406** | [Download](https://cyberduck.io/download/) |

The link opens the project's download page. The build at the top of it matches the version above and is untouched by this repository.

## Works with

- Windows 11 (x64)
- Windows 10 22H2 and newer
- macOS 12 and newer
- CLI companion `duck` for scripted transfers

## SHA-256 checksum

`SHA256SUMS.txt` in the releases tab carries the checksum of the current installer. Compare it locally:

```powershell
Get-FileHash -Algorithm SHA256 <path-to-installer>
```

## Install

1. Download the Windows installer from the download page.
2. Run it and accept the SmartScreen prompt — the package is unsigned on some builds.
3. Create a bookmark, choose the protocol and fill in the endpoint, keys and region.
4. There is no documented silent switch; for scripted rollouts use the `duck` CLI instead of the GUI.
5. Store the secret key only in the bookmark, not in a shared profile. Bookmarks live under `%APPDATA%\Cyberduck`, and that folder is what gets copied between machines.

## What it does

- S3, Swift, WebDAV, SFTP, FTP, Azure, Backblaze B2 and Google Drive in one client
- Client-side encryption with Cryptomator vaults
- ACL, metadata and versioning editors for object storage
- Resumable uploads and downloads with bandwidth throttling
- Bookmarks with per-connection profiles and external editors
- Transfer queue with retry and per-file progress
- Synchronise a local folder against a remote path

## Compatibility

Each protocol brings its own defaults, and Cyberduck shows the port it will use on the bookmark sheet: 21 for FTP, 22 for SFTP, 80 and 443 for WebDAV, 443 for the HTTPS-based object stores. S3 needs a region and a signature version; endpoints that predate the current signing scheme are still selectable from the protocol dropdown, which is where most "signature does not match" reports start.

SMB is not one of the supported protocols. Windows shares are mounted by the operating system, and Cyberduck has no path into them — that is a deliberate boundary rather than a missing feature, and no hidden setting changes it. The same applies to NFS.

Cryptomator vaults are opened, not created, by Cyberduck. The vault has to exist already, and the master key file stays with it, so a copied vault directory is useless without that file. Once unlocked, the vault behaves like a normal folder and the encryption happens before anything reaches the network.

Connection profiles are the extension point. A `.cyberduckprofile` file adds a provider with its own hostname, default port and OAuth endpoints, and dropping one into the application opens it as a new bookmark type. That is how smaller object stores get supported without waiting for a release.

## Troubleshooting

**"Connection failed" on a host that works elsewhere.** Check the protocol first: FTP on port 22 and SFTP on port 21 produce the same vague error. Then check TLS — servers offering only obsolete protocol versions are rejected, and the log drawer (**View → Log**) names the handshake failure.

**"Certificate not trusted" for a self-signed WebDAV server.** The certificate has to be accepted once per host, and the exception is stored per bookmark. Accepting it for the hostname does not cover the IP address, which is why the same server can work by name and fail by number.

**`SignatureDoesNotMatch` on S3.** Almost always clock skew or the wrong region. Windows time sync drifting by minutes is enough to invalidate a signed request; the region in the bookmark has to match the bucket, not the console default.

**"Listing directory failed" with a permission error.** The credentials are valid but the policy does not allow `ListBucket`. Granting read on objects alone lets a direct download work while the browser window stays empty, which reads like a broken connection.

**Large uploads restart from zero.** The connection dropped before a multipart part completed, or the endpoint does not support multipart at all. Enable resume in the transfer settings and check the part size against the provider's minimum.

## Alternatives

WinSCP is the natural choice for SFTP and SCP on Windows, with a scripting interface and a dual-pane layout. FileZilla covers FTP and FTPS with a similar queue model but no object-storage protocols. rclone is the command-line answer for synchronising buckets and does not need a GUI session. S3 Browser and similar tools go deeper into bucket policy editing, while Transmit fills the same role on macOS.

## FAQ

**Is Cyberduck free?** — GPLv3, no key and no activation.

**Does it run on Windows 11?** — Yes. The 9.6 line is built for Windows 10 22H2 and 11, and older builds remain on the download page.

**Can it be installed silently?** — Not with a documented switch. Package the installer, or move scripted work to the `duck` CLI.

**Can it mount a bucket as a drive letter?** — No. Cyberduck browses and transfers; mounting is left to the operating system or a separate tool.

**Does it edit files in place?** — It downloads to a temporary copy, opens the external editor, and uploads on save. Very large files make that cycle slow, and a crashed editor leaves the temporary copy behind.

**Where are passwords stored?** — In the bookmark file, obfuscated rather than encrypted unless a master password is set. Setting one is worth the prompt on every launch.

## Disclaimer

This page collects the installer link, version data and SHA-256 checksum for offline installation and archival. Product names and trademarks belong to their respective owners. No affiliation or endorsement is implied.
