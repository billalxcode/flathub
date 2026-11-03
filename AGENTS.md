# Agent guide: adding software to IGOS Nusantara Flathub

This repo holds the Flatpak apps IGOS Nusantara publishes at
https://r2.igos-nusantara.or.id/flatpak/ (remote file `igos.flatpakrepo`).
Everything is built and published by GitHub Actions; never upload to R2 by hand
(the R2 credentials only exist as repository secrets).

## Layout

```
apps/<app-id>/<app-id>.yml   flatpak-builder manifest (or .yaml/.json); the app ID is the dir name
apps/<app-id>/upstream       optional: "owner/repo" of a GitHub project to follow (see below)
apps/<app-id>/pypi           optional: PyPI package whose "<pkg>==<version>" pin the bot bumps
apps/<app-id>/*              patches, data files the manifest references
tools/update.sh              regenerates manifests of apps that have an upstream file
.github/workflows/build.yml  builds changed apps, signs and publishes them to R2
.github/workflows/update.yml daily upstream release check (03:00 WIB), commits and builds
```

## Adding an app

Pick the first case that fits.

1. **Upstream ships a Flatpak bundle manifest** (`packaging/linux/flatpak/*.bundle.yml`
   with a `type: dir` / `path: stage` source and a `*-linux-x86_64.tar.gz` release
   asset, like the storytold/ArtCraft apps): create `apps/<app-id>/upstream` with
   `owner/repo`, run `tools/update.sh <app-id>`, and commit both files. Don't edit
   the generated manifest; the daily Update workflow regenerates it on every new
   release. A different layout needs a change in `tools/update.sh`, not a hand edit.
2. **The app is on Flathub**: copy its manifest from `github.com/flathub/<app-id>`
   (with its patches and the files it references) into `apps/<app-id>/`.
3. **Otherwise** write a manifest. Prefer the freedesktop/GNOME/KDE runtimes from
   Flathub (the build installs them with `--install-deps-from=flathub`), pin every
   source with `sha256` or a git `commit`, keep `finish-args` minimal and comment
   each one, and ship a `.desktop`, icon and metainfo under the app ID.

Python apps from PyPI (example: `apps/si.biolab.orange`): base on
`com.riverbankcomputing.PyQt.BaseApp` for Qt, `pip3 install --prefix=${FLATPAK_DEST}
<pkg>==<version>` with `build-args: [--share=network]`, and a `pypi` file holding the
package name so the daily check bumps the pin.

Otherwise cases 2 and 3 are not covered by the daily check: bump their versions by hand.

## Checks before pushing

- The app ID is reverse-DNS, matches the directory and the manifest's `id:`.
- Build it locally when you can:
  `flatpak-builder --user --install-deps-from=flathub --force-clean --default-branch=stable --repo=/tmp/repo /tmp/build apps/<id>/<id>.yml`
- Only x86_64 is built for now.
- Licences allow redistribution (we host the binaries).

## Publishing

Push to `main`: the Build workflow rebuilds the apps whose directory changed
(`apps/**` paths), then commits them to the OSTree repo on R2 signed with the IGOS
repository key. To rebuild without a change, run Build by hand with
`gh workflow run build.yml -f app="<id> <id>"` (empty = all apps). Check the run
with `gh run list -w Build`.

Commits: no `Co-Authored-By` trailer for Claude; plain imperative subject lines.
