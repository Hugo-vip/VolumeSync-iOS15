name: Build RootHide DEB & TrollStore TIPA
on: [push, workflow_dispatch]
jobs:
  build:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install roothide/theos & Build
        run: |
          brew install ldid dpkg || true
          bash -c "$(curl -fsSL https://raw.githubusercontent.com/roothide/theos/master/bin/install-theos)"
          export THEOS="${HOME}/theos"
          export PATH="${HOME}/theos/bin:${PATH}"
          sed -i '' 's/VolumeSync_PRIVATE_FRAMEWORKS := Celestial MediaExperience/VolumeSync_FRAMEWORKS := UIKit Foundation/' build_volumesync.sh
          sed -i '' 's/#import <notify.h>/#import <notify.h>\n#import <dlfcn.h>/' build_volumesync.sh
          sed -i '' 's/float clamped/dlopen("\/System\/Library\/PrivateFrameworks\/Celestial.framework\/Celestial", RTLD_LAZY); dlopen("\/System\/Library\/PrivateFrameworks\/MediaExperience.framework\/MediaExperience", RTLD_LAZY); float clamped/' build_volumesync.sh
          chmod +x build_volumesync.sh
          ./build_volumesync.sh
      - uses: actions/upload-artifact@v4
        with:
          name: VolumeSync-iOS15.6-arm64e
          path: |
            VolumeSyncProject/roothide-tweak/packages/*.deb
            VolumeSyncProject/trollstore-app/VolumeSync.tipa
