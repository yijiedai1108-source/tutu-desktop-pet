# Shijima-Qt 自建版說明

- 基底：pixelomer/Shijima-Qt v0.1.0（官方已 archive，不再維護）
- Patch：
  - `shijima-dock-floor.patch` — 修 ShijimaManager.cc 中 taskbarHeight / statusBarHeight
    運算元對調的正負號 bug；修後地板 = Dock 上緣、天花板 = 選單列下緣。
  - `shijima-all-changes.patch` — 主 repo 的完整改造（桌寵功能 + 地板修正 + 下列所有 macOS 修正）。
    直接套在乾淨的 v0.1.0 上。
  - `libshijima-changes.patch` — **submodule `libshijima` 的改動**（速度倍率，見下）。主 repo 的 patch
    抓不到 submodule，需在 `git submodule update` 之後、進到 `libshijima/` 目錄單獨套用
    （檔頭第一行是 base commit 註解，用 `tail -n +2 libshijima-changes.patch | git apply -` 套）。
- 地板角落不裁切（`ShijimaWidget::updateOffsets`）：引擎原本會把「超出螢幕的幀」往螢幕外偏移繪製、
  讓超出部分被裁（地板走到角落身體被切一小條）。改成**只有站/走在地板上（`env->floor.is_on(anchor)`）
  且非拖曳時**，把該幀整個推回螢幕內（身體完整、邊緣抵住牆），只動水平方向。
  **爬牆(在 work_area 牆上)與天花板維持原本邏輯**——原本的往外偏移正是讓爬牆幀左側透明留白推出螢幕、
  身體剛好落在螢幕邊的關鍵，所以爬牆仍貼真邊。拖曳時也維持原狀（跟游標、可移出邊緣去別的螢幕）。垂直方向不變。
- 左右牆內縮（`ShijimaManager::updateEnvironment`）：可選的額外內縮，把 workArea / floor / ceiling
  左右邊往內移。**預設 0**（貼真邊；裁切已由上面 render 修法處理）。只縮「外緣」——
  某側若有相鄰螢幕（可把兔兔拖過去）則不縮該側。要讓兔兔離邊遠一點可用 defaults 微調（改完重開 app）：
  ```bash
  defaults write com.pixelomer.Shijima-Qt "wall.leftMargin"  -int 0     # 左牆內縮 px（預設 0）
  defaults write com.pixelomer.Shijima-Qt "wall.rightMargin" -int 0     # 右牆內縮 px（預設 0）
  ```
- macOS per-pixel 點擊穿透：原始碼把視窗遮罩（`Asset::mask` / `ShijimaWidget` 的 `setMask`）
  與 `Platform::useWindowMasks()` 從 `#ifdef __linux__` 放行到 `__APPLE__`，並讓 `useWindowMasks()`
  在 macOS 回傳 true。效果：兔兔只有實際身體像素會擋滑鼠，身體周圍透明區可穿透——
  貼著 App 側邊選單爬時，沒被身體蓋住的選單仍點得到（解決「牆內縮太多爬起來飄」與「擋選單」的兩難）。
  代價：遮罩是 1-bit alpha 門檻，身體邊緣會略比原本的平滑 alpha 硬一點點。
- 遮罩效能快取（`ShijimaWidget::paintEvent`）：上面的遮罩原本每幀都重算並 `setMask`（macOS 每幀重塑
  視窗有成本）。改成用 `m_maskAsset/m_maskMirror/m_maskOrigin/m_maskScale` 快取，**只在圖片幀/鏡像/
  位置/縮放變動時才重算**，兔兔單純移動的每幀跳過。
- 下班狂奔更亂（`ShijimaManager` tick）：原本每 tick 硬設 `next_behavior("RunAlongWorkAreaFloor")`，
  但引擎實際的行為名是**英文**的（經 translator/parser 產生，見右鍵 Behaviors 選單那份），
  而且跑/爬類多半 `hidden`。改成 20 秒慶祝期間每隔約 1.2~2.4 秒，
  從 `initial_behavior_list().flatten_unconditional()` 拿真實名，**忽略 hidden**、用白名單挑會動的
  （`RunAlongWorkAreaFloor` / `CrawlAlongWorkAreaFloor` / `WalkAndGrabBottomLeftWall` / `...RightWall`），
  指派前先確認該兔兔真的有該行為。
  - ⚠️ 大雷：`next_behavior(name)` 只是把名字塞進 `queued_behavior`，真正的 `find(name, throws=true)`
    發生在兔兔**下一個 tick**，找不到會丟 `logic_error`——該 throw 不在呼叫端 try/catch 內，會讓整個
    app `abort`。所以名字一定要是引擎實際存在的（別用 pack 的日文原名，會被 translator 改掉）。
  - 保險：`ShijimaManager` tick loop 的 `shimeji->tick()` 已包 try/catch，任何行為例外都不會再讓 app abort。
- 手動觸發下班狂奔：右鍵選單加了「🎉 下班狂奔（20 秒）」→ `ShijimaManager::startCelebration()`
  （設 `m_celebrateUntilMs`），不用等到下班時間也能測試/炫耀。
- 移動速度倍率（**libshijima patch**：`animation.cc` 走/爬 + `fall.cc` 掉落，乘 `env->speed_mult`；
  `environment.hpp` 加成員；`ShijimaManager::updateEnvironment` 從設定注入）。做法是「加大每步位移」而非
  「加快 tick」——動畫維持自然（不會快轉）。全方向一致。上限 3.5（再高會一步跨過邊界判定卡住）。
  ```bash
  defaults write com.pixelomer.Shijima-Qt "mascot.speed" -float 2.5   # 走/爬/掉速度倍率（0.2~3.5）
  ```
  程式內預設值已改為 **2.5**（= 定案手感；2026-07-16 改，原為 1.0）——Windows 沒有 defaults 指令，
  靠程式內預設值讓新安裝直接是定案手感。已設過的機器不受影響（設定值優先）。
- 跨平台通知（`ShijimaManager::notify`，2026-07-16 加，Windows 支援的一部分）：原本三處提醒
  （久坐、下班、肚子餓/喝水）直接呼叫 `osascript`，Windows 上會靜默失敗。抽成 `notify(message)`：
  macOS 維持 osascript 通知中心；其他平台 lazy 建一個 `QSystemTrayIcon`（🐰 圖示）用
  `showMessage()`——Windows 10/11 會顯示原生 toast。
- 自動跨螢幕：**嘗試過但已還原**。做法是「相鄰螢幕側打通邊界 + 非拖曳時 `screenAt` 切換環境」，
  但在「上下堆疊且偏移」的雙螢幕上，接縫 y=0 同屬兩螢幕會造成反覆切換/全體變慢，且爬牆跨接不順。
  目前維持原生的**拖曳跨螢幕**（把兔兔拖到另一個螢幕即可）。
- 建置（macOS arm64）：clone v0.1.0 → 套 `shijima-all-changes.patch` → `git submodule update --init --recursive`
  →（進 `libshijima/`）套 `libshijima-changes.patch` → brew install qt libarchive pkgconf cmake，然後
  `CONFIG=release make -j8 QT_MACOS_PATH=/opt/homebrew/opt/qt/lib CONFIG_LDFLAGS="-flto -Wl,-headerpad_max_install_names"`
  （submodule 的 SSH URL 要改 https；unarr 需 `CMAKE_POLICY_VERSION_MINIMUM=3.5`；
  libarchive keg-only 要設 `PKG_CONFIG_PATH=/opt/homebrew/opt/libarchive/lib/pkgconfig`）
- 建置（Windows x86_64）：**用 GitHub Actions**（`.github/workflows/build-windows.yml`，
  手動 dispatch 或改動 `patches/**` 推上 main 自動觸發）。流程照抄上游：clone v0.1.0 → 套兩份 patch →
  上游 `dev-docker/`（Fedora 42 + MinGW Qt6）Docker 交叉編譯 `mingw64-make` → 產物在
  `publish/Windows/release/`（exe + 全部 DLL），workflow 再把 `packs/usagi-shimeji` 壓成 zip、
  連同 `docs/README-windows.txt` 一起打成 `tutu-windows-x86_64` artifact。
  Windows 不需要遮罩/點擊穿透 patch（`useWindowMasks()` 回 false，Qt layered window 原生穿透）；
  地板 = 工作列上緣（跨平台 `availableGeometry` 算的）。設定存 registry
  （`HKCU\Software\pixelomer\Shijima-Qt` 一帶），一般人不用碰——速度預設已是 2.5。
- 打包：cp Shijima-Qt.app 骨架 + binary → `macdeployqt` → `codesign --force --deep -s -`
  - ⚠️ Makefile 的 `macapp` 目標把 macdeployqt 寫死成 MacPorts 路徑
    (`/opt/local/libexec/qt6/bin/macdeployqt`)，Homebrew 環境會失敗（binary 本身已編好）。
    Homebrew 用 `/opt/homebrew/bin/macdeployqt`，或沿用既有已 deploy 的 .app 骨架、只換 binary：
    把 binary 的 `@rpath`、Qt framework、libarchive 路徑用 `install_name_tool` 改回
    `@executable_path/../Frameworks`（libunarr 走 `@rpath/libunarr.1.dylib`），再重簽章即可。
