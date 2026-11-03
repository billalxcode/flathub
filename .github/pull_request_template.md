App ID:
Upstream (homepage / repository):
Licence (SPDX) and where upstream states it:

Checklist (see CONTRIBUTING.md):
- [ ] The licence allows us to redistribute the app; for proprietary apps, link the redistribution clause
- [ ] `<project_license>` in the metainfo matches the upstream licence
- [ ] The licence text is installed in the app
- [ ] Copyleft: the manifest points at the exact upstream source (tag/commit) of what we ship
- [ ] All sources are official upstream, pinned (sha256 / commit / ==version), HTTPS
- [ ] App ID follows upstream, or reverse-DNS of the upstream domain
- [ ] `.desktop`, icon and metainfo installed under the app ID
- [ ] Every `finish-args` entry has a comment; sensitive ones are justified below
- [ ] Telemetry/ads/online accounts are disclosed in the metainfo
- [ ] The Build workflow passes

Notes / justification for sensitive permissions:
