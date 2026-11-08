# Build UltraEdge firmware for your radio

Pick an UltraEdge version and your radio, click **Run**, and download firmware you can flash. It runs on GitHub's
servers, free, with nothing to install.

**Check [Releases](https://github.com/50UR4V/edgetx-ue/releases) first.** Most radios already have a ready-made file.
Build your own when you want another language, an extra option such as Bluetooth, or a version that isn't on the
Releases page for your radio.

## Before you start: UltraEdge is built on EdgeTX 3.0-dev

UltraEdge firmware is EdgeTX with the UltraEdge companion added. Every UltraEdge version is built on one fixed EdgeTX
version, shown next to it in the form. Today that is **EdgeTX 3.0-dev** (EdgeTX's development line from August 2026),
**not** EdgeTX 2.11 or 2.12. You can't choose a different EdgeTX version: UltraEdge only works on the one it was
made for.

EdgeTX updates your models and settings on the SD card when you flash a newer EdgeTX. **Back up your SD card before
you flash.** If you later go back to stock EdgeTX 2.x, restore that backup.

## Build it (about 15 minutes)

You need a free GitHub account.

1. On this page, click **Use this template ▸ Create a new repository**. Keep it **Public**: public repositories
   build for free.
2. In your new repository, open the **Actions** tab. If GitHub asks, click **I understand my workflows, go ahead and
   enable them**.
3. Click **Build UltraEdge firmware ▸ Run workflow** and choose:
   - **UltraEdge version.** The current release, shown with the EdgeTX version it is built on. Production and beta
     apps use the same firmware; pair it with the app version listed for it on the Releases page.
   - **Radio.** Radios marked **(community-unverified)** build fine, but nobody has checked them on the real radio
     yet. See below.
   - **Radio menus.** Leave it on *default*. Black-and-white radios then get the companion build (model and radio
     settings are made in the app, which frees the flash space UltraEdge needs); colour radios keep every menu.
   - **Language**, **Version label** and **Advanced options**: optional.
4. Click **Run workflow**. When the run turns green, open it and download **UltraEdge-fw-<radio>** under
   **Artifacts**. The zip holds the firmware (`.bin`, or `.uf2` for H750 radios) and a `.txt` describing it.
5. Flash it as described in [FLASHING.md](https://github.com/50UR4V/edgetx-ue/blob/ultraedge/FLASHING.md).

If the run turns red, its summary says why. "Doesn't fit" means that radio has too little flash for this version and
options: the companion menus or fewer advanced options.

## Community-unverified radios

[SUPPORTED-RADIOS](https://github.com/50UR4V/edgetx-ue/blob/ultraedge/SUPPORTED-RADIOS.md) is the list of which
radios have been checked on real hardware. A radio marked community-unverified there has only been built, not flown.
If you have one:
1. Flash it on the bench and check that the radio boots, the sticks and switches move the outputs, and your model
   binds, before you fly.
2. Plug in the UltraEdge app and check that it connects, shows telemetry and edits a model.
3. Report what worked and what didn't in [UltraEdge-app issues](https://github.com/50UR4V/UltraEdge-app/issues), with
   your radio and the `.txt` from your build. Passing reports move the radio to verified.

Flying a build you made yourself is your call.

## How to tell it's yours

The radio's Version page and the app's radio details show `UE <version>-selfbuild` (or your label), the UltraEdge
source commit, and the date you built it. Put that line in bug reports. If you share your build, share the `.txt`
with it: it names the public source it was built from, which the GPL asks you to point to.

## In a terminal instead (Codespaces, Docker or your own machine)

Open a Codespace on the firmware source with the UltraEdge build config:
`https://codespaces.new/50UR4V/edgetx-ue?ref=ultraedge&devcontainer_path=.devcontainer/ultraedge/devcontainer.json`
Then:

```bash
tools/ultraedge/ue-build.sh list            # radios this version builds, and which are verified
tools/ultraedge/ue-build.sh tx16s           # build; the firmware lands in out/
tools/ultraedge/ue-build.sh tx16s --lang DE --opt -DBLUETOOTH=YES
```

Right-click the file in `out/` ▸ **Download**. The same script works in EdgeTX's Docker image
(`ghcr.io/edgetx/edgetx-dev`) and on a machine with the Arm GNU Toolchain, CMake and Python.

## Licence

The build recipe in this repository is GPL-2.0-only, like UltraEdge's firmware overlay and EdgeTX itself. This is an
UltraEdge project, not an EdgeTX service; EdgeTX's own firmware is at <https://github.com/EdgeTX/edgetx>.
