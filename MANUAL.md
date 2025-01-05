# Building Manual using Docker


First install docker in your system and then use this docker [image](https://github.com/mlm-games/android-builder-AOSP-7.1)


#### Install clone repo in bin and add it to path:

[Reference](https://wiki.lineageos.org/devices/bacon/build/#install-the-repo-command)

#### Initialize Repo (on the host machine or by using python3 in docker):
```
mkdir ~/workdir && cd ~/workdir
repo init -u https://github.com/LineageOS/android.git -b cm-14.1
```
#### Sync sources (it may require some hours)
```
repo sync -c -q -j8 --force-sync --no-clone-bundle --no-tags --optimized-fetch --prune
``` 
#### Clone necessary trees
```

git clone https://github.com/mlm-games/android_device_motorola_nicklaus -b cm-14.1 device/motorola/nicklaus --depth=1
git clone https://github.com/LineageOS-MediaTek/android_device_mediatek_mt6737-common -b cm-14.1 device/mediatek/common --depth=1
git clone https://github.com/LineageOS-MediaTek/proprietary_vendor_motorola -b cm-14.1 vendor/motorola --depth=1
git clone https://github.com/Klozz/android_kernel_motorola_nicklaus kernel/motorola/nicklaus --depth=1

```
## Building Source

#### Flags for disabling dexpreopt stuff (maybe not needed)
```
export USE_DEX2OAT_DEBUG=false
export WITH_DEXPREOPT=false
export WITH_DEXPREOPT_BOOT_IMG_ONLY=false
export DISABLE_DEXPREOPT=true
export PRODUCT_MINIMIZE_JAVA_DEBUG_INFO=true
export TEMPORARY_DISABLE_PATH_RESTRICTIONS=true
```

#### Enable ccache for Improve speed of build
```
export CCACHE_DIR=./.ccache
ccache -C
export USE_CCACHE=1
export CCACHE_COMPRESS=1
prebuilts/misc/linux-x86/ccache/ccache -M 50G
```
#### Clean your build
```
make clean && make clobber
```

#### Start building :) 
```
. build/envsetup.sh 
breakfast nicklaus
make -j8 bacon
```
