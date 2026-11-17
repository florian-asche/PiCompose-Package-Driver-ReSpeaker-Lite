# Abweichungen von der Stage `02-stage-audiodriver-respeaker_lite`

Dieses Dokument beschreibt, was das Paket `picompose-driver-respeaker-lite` anders macht als die gleichnamige Stage im [PiCompose](https://github.com/florian-asche/PiCompose)-Repository. Verglichen wurde mit dem Stand von `main` (Commit `784abfb`, 17.11.2026). Gepflegt wird es auf Deutsch, auf Wunsch des Entwicklers.

Pflege: Jede Änderung, die eine Abweichung neu schafft, ändert oder beseitigt, aktualisiert dieses Dokument. Prüfe dabei gegen den aktuellen `main` von PiCompose.

## Anders gelöst

- Die Unit-Datei liegt in `/usr/lib/systemd/system/` statt in `/etc/systemd/system/`. Aktiviert wird sie durch `dh_installsystemd` im Paket (beim Installieren aktiviert, aber nicht gestartet) statt durch `systemctl enable` im Chroot der Stage. Der Inhalt der Unit ist unverändert. Das gilt für `configure_audio.service` und `keep-audio-alive.service`.

## Zusätzlich im Paket

- `Depends` auf `sox` (wie `01-keep-audio-alive/01-packages`) und zusätzlich auf `libsox-fmt-alsa` (für die ALSA-Ausgabe von `sox`) und `pipewire-bin` (`pw-cli`). Diese beiden stehen nicht in der Paketliste der Stage.
- `Depends` auf `wireplumber` (`wpctl`) und `alsa-utils` (`amixer`, `alsactl`).
- `Provides`, `Conflicts` und `Replaces` für `picompose-audio-driver`.

## Unverändert

- `keep-audio-alive.sh`, `keep-audio-alive.service`, `configure_audio.sh` und `configure_audio.service` sind Byte für Byte die Dateien der Stage.
