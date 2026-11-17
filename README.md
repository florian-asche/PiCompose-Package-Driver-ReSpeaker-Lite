# PiCompose-Package-Driver-ReSpeaker-Lite

Package `picompose-driver-respeaker-lite` for the [PiCompose APT repository](https://github.com/florian-asche/PiCompose-Repository). The ReSpeaker Lite needs no kernel driver. It is the stage `02-stage-audiodriver-respeaker_lite` of the [PiCompose](https://github.com/florian-asche/PiCompose) image as a package.

## Contents

| Part | Files |
| --- | --- |
| Keep the card awake | `/usr/bin/keep-audio-alive.sh`, `keep-audio-alive.service` (enabled, not started at install) |
| Volume setup | `/usr/bin/configure_audio.sh`, `configure_audio.service` (enabled, not started at install) |

`keep-audio-alive.service` waits for PipeWire and plays silence with `sox` all the time, so the sound card does not go to sleep. `configure_audio.service` sets the volume of the default sink to 100 % at start and stores the ALSA state.

The files are the ones of the image without changes, and both services use the user with the ID 1000 (`/run/user/1000`), as the image does.

## One driver per system

All `picompose-driver-*` packages provide `picompose-audio-driver` and conflict with it. Installing one removes another, because a system has one audio device and all of them install `configure_audio.sh` and `configure_audio.service`.

## Versioning and changelog

The version of the package is the first entry of `debian/changelog`, which is part of the package. You do not edit it by hand, the workflows write it.

1. **Pull request:** Every pull request needs exactly one of the labels `major`, `minor` and `patch`. Tick one box under "Version bump" in the description or set the label by hand. The check "Version label" fails until there is exactly one.
2. **Merge to `main`:** The workflow "Version" raises the version, adds the pull request titles to `debian/changelog`, commits it to `main`, creates the tag `v<version>` as pre-release and starts the build, which publishes to the suite `trixie-nightly`.
3. **Release:** Turn the pre-release into a release in the GitHub UI. That publishes the same version to the suite `trixie`.
4. **Any other branch:** `<version>+dev<timestamp>.<sha>`, kept as workflow artifact for 14 days, nothing is published.

The repository needs the secret `HUB_DISPATCH_TOKEN` and a branch protection of `main` that requires the check "Version label" and lets `github-actions[bot]` push, see the [DEVELOPMENT.md](https://github.com/florian-asche/PiCompose-Repository/blob/main/DEVELOPMENT.md) of the APT repository. The repository also has to be listed in `sources.yml` of the APT repository.

## Local build

```sh
sudo apt install build-essential debhelper devscripts fakeroot lintian
dpkg-buildpackage -us -uc -b
lintian ../picompose-driver-respeaker-lite_*.deb
```

## License

This project is released under the [MIT License](LICENSE).
