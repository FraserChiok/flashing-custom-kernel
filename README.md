# Flashing NVIDIA Jetson Orin NX 16GB with Custom Kernel Configuration

Before flashing your NVIDIA Jetson Orin NX 16GB device with a custom kernel configuration, follow the steps below:

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

Now, your NVIDIA Jetson Orin NX 16GB device is ready for flashing with the custom kernel configuration.
