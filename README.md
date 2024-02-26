# Flashing NVIDIA Jetson Orin NX 16GB with Custom Kernel Configuration

Before flashing your NVIDIA Jetson Orin NX 16GB device with a custom kernel configuration, make sure you have the following prerequisites:

1. **NVIDIA Jetson Orin NX 16GB device:** Ensure you have the Jetson Orin NX 16GB development kit.
   
2. **Custom Kernel Configuration:** Have a custom kernel configuration ready according to your requirements.

3. **Toolchain Installation:** Install the appropriate toolchain for cross-compilation. You can download the toolchain from the [NVIDIA Jetson Linux Toolchain Documentation](https://docs.nvidia.com/jetson/archives/r35.1/DeveloperGuide/text/AT/JetsonLinuxToolchain.html). Ensure the toolchain's bin directory is in your `PATH` environment variable.

4. **Compilation Resources:** Download any additional resources required for compilation from the [NVIDIA Embedded Downloads](https://developer.nvidia.com/embedded/downloads) page.

Now, follow the steps below to flash your device with the custom kernel configuration:

1. Set up your environment variables:
    ```bash
    export CROSS_COMPILE_AARCH64=/home/ubu/l4t-gcc/bin/aarch64-buildroot-linux-gnu-
    export CROSS_COMPILE_AARCH64_PATH=/home/ubu/l4t-gcc

    export CROSS_COMPILE=$HOME/l4t-gcc/bin/aarch64-buildroot-linux-gnu-
    export CROSS_COMPILE_AARCH64_PATH=<toolchain-path> (/home/ubu/l4t-gcc)
    export CROSS_COMPILE_AARCH64=<toolchain-path>/bin/aarch64-buildroot-linux-gnu-
    export TOP=<top_working_directory_> (/home/ubu/test)
    ```

2. Move `public_sources.tbz2` to your top working directory:
    ```bash
    mv public_sources.tbz2 $TOP
    ```

3. Extract the public sources:
    ```bash
    cd $TOP
    tar -xjf public_sources.tbz2
    ```

4. Navigate to the kernel source directory:
    ```bash
    cd /$TOP/Linux_for_Tegra/source/public
    tar -xjf kernel_src.tbz2
    ```

5. Create a directory for kernel output:
    ```bash
    mkdir kernel_out
    ```

6. Enter the kernel directory:
    ```bash
    cd $TOP/Linux_for_Tegra/source/public/kernel/kernel*
    ```

7. Backup the default tegra_defconfig:
    ```bash
    cp $TOP/Linux_for_Tegra/source/public/kernel/kernel-5.10/arch/arm64/configs/tegra_defconfig $TOP/Linux_for_Tegra/source/public/kernel/kernel-5.10/arch/arm64/configs/tegra_defconfig.backup
    ```

8. Configure the kernel using tegra_defconfig:
    ```bash
    make ARCH=arm64 tegra_defconfig
    ```

9. Modify the tegra_defconfig according to your requirements:
    ```bash
    CONFIG_SECURITY_APPARMOR=y
    CONFIG_SECURITY_APPARMOR_HASH=y
    CONFIG_SECURITY_APPARMOR_HASH_DEFAULT=y
    CONFIG_DEFAULT_SECURITY_APPARMOR=y
    CONFIG_LSM="lockdown,yama,loadpin,safesetid,integrity,apparmor,selinux,bpf"
    ```

10. For enabling ISCSI support, adjust the following configurations in tegra_defconfig:
    ```bash
    CONFIG_CRYPTO=y
    CONFIG_ARM64_CRYPTO=y
    CONFIG_CRYPTO_CRC32C=y
    CONFIG_ISCSI_TCP=m
    CONFIG_SCSI_ISCSI_ATTRS=m
    CONFIG_SCSI_LOWLEVEL=y
    CONFIG_BLK_DEV=y
    CONFIG_INET=y
    ```

Now, your NVIDIA Jetson Orin NX 16GB device is ready for flashing with the custom kernel configuration.
