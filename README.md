# Device Tree for ASUS ZenFone6 (I01WD)

The ASUS ZenFone6 (codenamed _"I01WD"_) is a flagship smartphone from ASUS.
It was released in May 2019.




## Compile

First download omni-9.0 tree:

```
repo init --depth=1 -u https://github.com/omnirom/android.git -b android-9.0
```
Then add these string to .repo/manifests/remove.xml

```
<remove-project name="platform/bootable/recovery" />
```

Then add these projects to .repo/local_manifests/roomservice.xml (If you don't have it, you can add them to .repo/manifest.xml): 

```xml
<project name="x86cpu/android_device_asus_I01WD_TWRP" path="device/asus/I01WD" remote="github" revision="android-9.0" />
<project name="mauronofrio/android_bootable_recovery" path="bootable/recovery" remote="github" revision="android-9.0" />
<project name="android_external_busybox" path="external/busybox" remote="TeamWin" revision="android-9.0" />
```

Now you can sync your source:

```
repo sync
```

To auotomatic make the twrp installer, you need to import this commit in the build path: https://gerrit.omnirom.org/#/c/android_build/+/33182/


To make all works you need to modify the buildinfo.sh in build/tools
echo "ro.build.version.release=$PLATFORM_VERSION"
echo "ro.build.version.security_patch=$PLATFORM_SECURITY_PATCH"
to
echo "ro.build.version.release_orig=$PLATFORM_VERSION"
echo "ro.build.version.security_patch_orig=$PLATFORM_SECURITY_PATCH"

And you need to increase the PLATFORM_VERSION to 16.1.0 in build/core/version_defaults.mk to override Google's anti-rollback features (This actually i don't know if is always needed)

Finally execute these:

```
. build/envsetup.sh
export ALLOW_MISSING_DEPENDENCIES=true
export LC_ALL=C
lunch omni_I01WD-eng
mka adbd recoveryimage 
```

To test it:

```
fastboot boot out/target/product/I01WD/recovery.img
```

Kernel Source: using precompiled kernel
## Credits
I want to say a big thanks to @mauronofrio
