# IGOS Nusantara Flathub

Flatpak apps maintained by IGOS Nusantara, published at
https://r2.igos-nusantara.or.id/flatpak/

```sh
flatpak remote-add --if-not-exists igos https://r2.igos-nusantara.or.id/flatpak/igos.flatpakrepo
flatpak install igos <app-id>
```

## Adding or updating an app

Put the flatpak-builder manifest at `apps/<app-id>/<app-id>.yml` (or `.yaml`/`.json`),
with its patches and data files beside it, and push to `main`. The Build workflow
rebuilds every app whose directory changed and publishes it (branch `stable`).
Run it by hand from the Actions tab to rebuild one app or all of them.
