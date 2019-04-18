
# Compiling with android ndk
```
export NDK_PATH=</work/ndk>
export TOOLCHAIN=/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/bin/arm-linux-androideabi-gcc
"${NDK_PATH}${TOOLCHAIN}" -Os bootmenu.c -o bootmenu -static -Wl,--gc-sections -Wl,--strip-all --sysroot=$NDK_PATH/platforms/android-13/arch-arm/
```

# Unpacking / repacking a boot.img

### Extract boot.img
```
abootimg -x boot.img
```

### Extract initrd.img
```
mkdir initrd
cd initrd
cat ../initrd.img | gunzip | cpio -vid
```

### Package initrd.img
```
find . | cpio --create --format='newc' | gzip > ../initrd_new.img
```

### Package boot.img
```
abootimg --create boot_new.img -f bootimg.cfg -k zImage -r initrd_new.img
```
