# Experimental elish ReSukiSU + SUSFS 2.3.0

This branch starts at the tested maintenance kernel 40744d7f6c534f06bdd62df6125642d6a095e3b4 for LineageOS 23.2-20260414-UNOFFICIAL-elish. A successful build does not confirm that the SUSFS variant boots on the tablet.

The build uses the same ROM configuration, device drivers, ZyC-Clang 15.0.7 toolchain, ReSukiSU revision 83850e8e93c5f7cfaec69cf03707f13e799df36a and AnyKernel3 revision 1ae369a5004c1a577e9d1aa68f49d0c4f8627fc6. KPM, KPROBES, LTO, CFI and shadow call stack remain disabled. SUSFS replaces the mutually exclusive manual hook mode.

## Patch provenance and adaptation

`patches/elish-susfs-2.3.0.patch` ports only the SUSFS integration from [AstideLabs commit 0b8a115](https://github.com/AstideLabs/android_kernel_xiaomi_sm8250/commit/0b8a115ddd4125dbda533a607f8de8ef2a08d56e). The upstream commit credits simonpunk, backslashxx, JackA1ltman and ApartTUSITU; original SUSFS development is at https://gitlab.com/simonpunk/susfs4ksu. No other AstideLabs driver changes are included.

The patch adapts context to the LineageOS baseline, replaces the old manual hooks with SUSFS inline hooks, uses the pinned ReSukiSU three-argument read hook, handles execve without a filename, runs post-exec hooks before releasing filenames, checks successful fstat before modifying its result, and fixes the KSTAT ctime seconds bit mask (`1 << 8`). The build checks the patch SHA-256 and refuses context failures. The original source baseline remains reviewable beside the patch.

The artifact contains the flashable AnyKernel3 ZIP, final kernel.config and build-info.txt. The installer replaces the current slot's boot kernel and preserves the ROM ramdisk, vendor_boot and dtbo. Keep a backup of the working boot partition before trying this experimental build. Root hiding depends on userspace settings and modules; compiling SUSFS does not guarantee passing integrity checks.
