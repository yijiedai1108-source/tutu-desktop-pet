# Shijima-Qt 自建版說明

- 基底：pixelomer/Shijima-Qt v0.1.0（官方已 archive，不再維護）
- Patch：`shijima-dock-floor.patch` — 修 ShijimaManager.cc 中 taskbarHeight / statusBarHeight
  運算元對調的正負號 bug；修後地板 = Dock 上緣、天花板 = 選單列下緣。
- 建置（macOS arm64）：brew install qt libarchive pkgconf cmake，然後
  `CONFIG=release make -j8 QT_MACOS_PATH=/opt/homebrew/opt/qt/lib CONFIG_LDFLAGS="-flto -Wl,-headerpad_max_install_names"`
  （submodule 的 SSH URL 要改 https；unarr 需 `CMAKE_POLICY_VERSION_MINIMUM=3.5`；
  libarchive keg-only 要設 PKG_CONFIG_PATH）
- 打包：cp Shijima-Qt.app 骨架 + binary → `macdeployqt` → `codesign --force --deep -s -`
