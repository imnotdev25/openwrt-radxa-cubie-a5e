# openwrt-radxa-cubie-a5e

OpenWrt build for the Radxa Cubie A5E (Allwinner A527, kernel 6.18). Wi-Fi uses the AIC8800D80 SDIO driver.

## Workflows

| Workflow | Contents |
|---|---|
| 01 Wi-Fi and Docker | Wi-Fi driver, hostapd/wpad, Docker stack, Dropbear, grow-rootfs tool |
| 02 Full image | Everything in `configs/full.config`: all themes (Argon, Material, Bootstrap, OpenWrt), Tailscale, cloudflared, AdGuard Home, Samba 4, openNDS, and about 690 packages built in |
| 03 Packages zip | Builds the full package set and zips `bin/packages/aarch64_cortex-a55` |

Trigger any workflow from the Actions tab (`workflow_dispatch`).

## Storage and runner requirements

- Disk: 120 GB free minimum. Build tree, downloads and output reach about 100 GB.
- CPU: 16 cores recommended.
- RAM: 30 GB recommended.
- Final image: the 16 GB root partition is set by `CONFIG_TARGET_ROOTFS_PARTSIZE=16384`. The `.img.gz` is about 420 MB.
- GitHub-hosted runners have about 14 GB free and cannot run these jobs. Register a self-hosted runner with the labels `self-hosted, linux, x64, openwrt-large`.

## Not in this repo

- Signing keys (`private-key.pem`, `public-key.pem`) are excluded. Keep them out of git.
- Root password is set on the device over UART with `passwd`.

## Known gaps

- Bluetooth is not working (device-tree patch 9021 is missing).
- USB Wi-Fi drivers are not included.
- The initramfs image is still built in `full.config`.
- The Wi-Fi and Docker seed does not include the `files/` overlay packages (openNDS, LuCI commands). Those first-boot scripts may fail on that variant.
