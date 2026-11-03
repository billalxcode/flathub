# Contributing

IGOS Nusantara hosts and distributes every app in this repository from its own
servers, signed with the IGOS repository key. That makes IGOS the distributor in
the legal sense, so an app is only accepted when we are clearly allowed to
redistribute it and users can see what they are installing. These rules apply to
people and to agents alike; [AGENTS.md](AGENTS.md) explains the mechanics.

## 1. Licence

- **The licence must be explicit.** The upstream project states it in a LICENSE
  or COPYING file, or in its package metadata. "Free download", a missing licence
  file, or "source available" without a licence is not enough: no licence means
  no permission to redistribute, and the app is rejected.
- **Free and open-source software first.** Licences approved by the OSI or the
  FSF (GPL, LGPL, MPL, Apache, MIT, BSD, ...) are accepted without discussion.
- **Proprietary or freeware apps** are accepted only when their licence (EULA)
  explicitly allows third parties to redistribute the binaries. Link the clause
  in the pull request. Without that clause, package them as an `extra-data`
  download from the vendor instead of hosting the binary. Use
  `LicenseRef-proprietary` (or `LicenseRef-proprietary=<URL of the EULA>`).
- **Declare it with SPDX** in the metainfo: `<project_license>GPL-3.0-or-later</project_license>`,
  `MIT OR Apache-2.0`, and so on (https://spdx.org/licenses/). It must match the
  upstream licence, including "or later" and dual licensing.
- **Ship the licence text** in the app: under `/app/share/licenses/<app-id>/`
  (or the `/app/share/doc/<app>/` the upstream package installs). CI rejects a
  build without a `<project_license>` or without a licence file.
- **Copyleft (GPL, LGPL, AGPL, MPL)**: the corresponding source must stay
  available. The manifest has to point at the exact upstream source, a release
  tag or commit, that the shipped binary was built from. Apps repackaged from
  prebuilt binaries are only acceptable when upstream publishes the source of
  that same release.
- **Licence changes**: when upstream changes its licence, the maintainer of the
  app re-checks these rules before the update goes out. The daily update bot does
  not judge licences, so anyone who spots a change opens an issue.

## 2. Origin of the software

- Sources and binaries come only from **official upstream locations**: the
  project's own release page, Git repository, PyPI/crates.io/npm package or
  download server. No unofficial mirrors, re-uploads, or files from forums.
- Every source is **pinned and verifiable**: `sha256` for archives and files,
  `tag` plus `commit` for Git, `==<version>` for pip. HTTPS only.
- Prefer building from source. Repackaging an official upstream binary is fine
  when the upstream build is the one users are expected to run (as with the
  ArtCraft apps).
- No cracked, patched-to-unlock, or otherwise license-circumventing builds.
  Nothing illegal to distribute in Indonesia.

## 3. Identity and trademarks

- **App ID**: use upstream's own ID when it has one (its Flathub ID, or the ID in
  its metainfo). Otherwise reverse-DNS of the upstream project's domain
  (`si.biolab.orange` for biolab.si). `id.or.igos-nusantara.*` is reserved for
  software IGOS itself develops.
- Keep upstream's **name, icon and branding** unchanged and do not suggest that
  upstream endorses IGOS. Say in the metainfo description, or a note, when the
  package is unofficial.

## 4. Metadata

Each app installs:
- a `.desktop` file named `<app-id>.desktop`;
- an icon named `<app-id>` (SVG, or PNG of at least 128×128);
- a metainfo file `<app-id>.metainfo.xml` with `id`, `name`, `summary`,
  `description`, `metadata_license` (CC0-1.0 or FSFAP), `project_license`,
  `developer`, `url type="homepage"`, `launchable` and `content_rating`.
  Use upstream's metainfo when it has one.

## 5. Permissions

- Ask for the **least** the app needs, and write a comment above every
  `finish-args` entry saying why.
- These need an explicit justification in the pull request and a maintainer's
  approval: `--filesystem=host` or `host-os`, `--device=all`, `--talk-name=org.freedesktop.Flatpak`
  (a sandbox escape), `--socket=system-bus`, `--socket=session-bus`, and any
  access to the user's credentials or SSH/GPG keys.
- Network access during the build (`build-args: --share=network`) is allowed,
  but everything it downloads must be pinned to exact versions.

## 6. Privacy and safety

- Telemetry, ads, or online accounts must be described in the metainfo
  description. Telemetry should be off by default or opt-out in the app.
- No malware, cryptominers, or bundled adware. When in doubt, don't add the app.

## 7. Process

1. Open a pull request (do not push to `main` directly; the update bot is the
   only exception). One app per pull request.
2. Fill in the checklist in the pull request template.
3. The Build workflow must pass for the app.
4. A maintainer reviews the licence, the origin, and the permissions, then merges.
   Merging publishes the app.

An app is removed when its licence no longer allows redistribution, upstream is
discontinued and it has security problems, or its build stays broken for 30 days.
