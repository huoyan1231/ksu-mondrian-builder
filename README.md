# KSU for mondrian

# I haven't updated the README; some of the information in it is outdated.

## workflow

The CI workflow builds kernel module files (`.ko`) for **mondrian** (Poco F5 Pro) against two kernels and three LKMs, then publishes them to GitHub releases.

| Kernel                 | KernelSU           | KernelSU-Next           | SukiSU           |
|------------------------|--------------------|-------------------------|------------------|
| **LineageOS**          | `kernelsu.ko`      | `kernelsu_next.ko`      | `sukisu.ko`      |
| **nyxx** (LOS+patches) | `nyxx_kernelsu.ko` | `nyxx_kernelsu_next.ko` | `nyxx_sukisu.ko` |

## ksupatcher-mondrian

this script is used for the actual kernel patching part, since some KSU managers dont allow the usage of custom `.ko` files for LKM integration. usage examples below

### new boot image patching

1. Put your `boot.img` in your phone's internal storage, you can get the boot image [here](https://download.lineageos.org/devices/mondrian/builds)
2. Download the script and put it in your phone's temp dir - `adb push ksupatcher-mondrian.sh /data/local/tmp/`
3. Enter the adb shell - `adb shell`
4. In the shell, execute the script:

   ```sh
   cd /data/local/tmp && chmod +rwx ksupatcher-mondrian.sh
   ```

5. Patch your boot image with your preferred LKM:

   ```sh
   ./ksupatcher-mondrian.sh ksun        # KernelSU-Next
   ./ksupatcher-mondrian.sh ksu         # KernelSU
   ./ksupatcher-mondrian.sh sukisu      # SukiSU
   ```

6. The boot image is now in downloads — transfer it to your computer:

   ```sh
   adb pull /sdcard/Download/kernelsu_next_patched_12345678_12345678.img
   ```

7. Reboot the phone to bootloader - `adb reboot bootloader`
8. Flash the new boot image - `fastboot flash boot kernelsu_next_patched_12345678_12345678.img`
9. Reboot your phone, install the root manager, and you're done!

### OTA patching

1. Finish downloading and installing an OTA, you have to **see** the reboot button, do not click it
2. Download the script and put it in your phone's temp dir - `adb push ksupatcher-mondrian.sh /data/local/tmp/`
3. Enter the adb shell - `adb shell`
4. In the shell, execute the script:

   ```sh
   cd /data/local/tmp && chmod +rwx ksupatcher-mondrian.sh
   ```

5. Patch your new boot image (extracted from the phone):

   ```sh
   ./ksupatcher-mondrian.sh ksun ota
   ```

6. After the script finishes, you can safely reboot the phone (and keep your root access intact)!

## thanks

I would like to thank cyberknight777 for their work [here](https://t.me/motorolag54updates/247), which was the inspiration for this script
