# NOTICE: Debian 13 KDE Plasma Desktop in Your Browser template

## Base image and its components

The service is built `FROM lscr.io/linuxserver/webtop:debian-kde-8ef9e32c-ls204@sha256:c5f9ce2a045d9bf4ed34993ad72c327c4b66e2e97ca8b021af9f95f1c70ea46a` (built 2026-11-03). The image is used as published by linuxserver.io. This repository contains no copy of its source files.

| Component | Source | License | How it is used |
|---|---|---|---|
| linuxserver.io `webtop`, release `debian-kde-8ef9e32c-ls204` | https://github.com/linuxserver/docker-webtop/tree/debian-kde (git revision `aab5a35c1cbcc6847ae735fbf28fc8213b380b6e`) | GPL-3.0 | Base image (`FROM`), unmodified except for the build-time edits listed below |
| linuxserver.io baseimage-selkies (`baseimage-selkies:debiantrixie`) | https://github.com/linuxserver/docker-baseimage-selkies | GPL-3.0 | Layers of the base image: the s6 service definitions, `init-nginx`, `/defaults/default.conf` |
| Selkies | https://github.com/selkies-project/selkies | MPL-2.0 | Streaming server and web client, in the base image |
| s6-overlay 3.2.1.0 | https://github.com/just-containers/s6-overlay, https://skarnet.org/software/ | ISC | Process supervision. `railway-start` calls `s6-svscan`, `s6-rc-compile`, `s6-rc-init` and `s6-rc` from the image |
| catatonit | https://github.com/openSUSE/catatonit (distribution package in the base image) | GPL-2.0-or-later | Container init and subreaper (`ENTRYPOINT`) |
| Debian 13 "trixie" packages: KDE Plasma, KWin, Dolphin, Konsole, Chromium, nginx, PulseAudio, Mesa and others | https://sources.debian.org/ | Each package's own license (`/usr/share/doc/*/copyright` in the image) | In the base image |

### GPL notes

- The GPL parts of the base image stay under their licenses. Their complete corresponding source is published by their authors at the repositories above, at the git revision named in the image label.
- **Modified files:** at build time, `services/desktop/patch-image.sh` edits `/defaults/default.conf` (GPL-3.0, from docker-baseimage-selkies: basic auth switched on, an Origin check, anti-framing headers and a `/healthz` location) and `/etc/nginx/nginx.conf` (2 worker processes, error log on stderr). The script and the inserted snippets (`nginx-origin-map.conf`, `nginx-server-hardening.conf`) are the complete record of these changes. Anyone who distributes a built image of this template should keep them available with it.
- The files written for this template (`Dockerfile`, `railway-start.sh`, `pkg-restore.sh`, `patch-image.sh`, the two nginx snippets and the documentation) are offered under GPL-3.0-or-later.
- No source file of linuxserver.io, Selkies or s6-overlay was copied into this repository. `railway-start` re-implements, in its own code, the order of steps that s6-overlay's `preinit`, `stage0` and `rc.init` perform, and then calls the image's own s6 tools and service definitions.

## Market templates reviewed

`linux-desktop-debian` "Linux Desktop | Debian KDE Plasma in the Cloud" (two-workspaces/webtop-debian, no license): read for ideas only. Nothing copied.

Product names belong to their owners. This is a community template, not affiliated with linuxserver.io, the Selkies project, the distribution's publishers or Railway. "Debian" is a registered trademark of Software in the Public Interest, Inc. KDE® and Plasma are trademarks of KDE e.V.
