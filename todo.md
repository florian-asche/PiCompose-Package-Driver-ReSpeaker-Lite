# To do for PiCompose-Package-Driver-ReSpeaker-Lite

Open points and work that was postponed. Remove an entry in the change that settles it.

1. **Test on hardware:** Install on Raspberry Pi OS Trixie with a ReSpeaker Lite and check both services. Nothing was tested on hardware.

2. **Is keep-audio-alive still needed:** The service plays silence as a workaround for the card going to sleep. Check whether the current PipeWire and WirePlumber still need it.

3. **Test the volume on hardware:** `configure_audio.service` was removed, the default volume of 100 % now comes from `device.routes.default-sink-volume` in `picompose-core`. Check on a ReSpeaker Lite with a fresh `~/.local/state/wireplumber` that the sink starts at 100 %. A card with an already stored (wrong) state keeps that value, the package no longer resets it.
