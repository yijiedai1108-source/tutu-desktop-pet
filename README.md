# 🐰 兔兔桌寵（Usagi Desktop Pet）

一隻住在你桌面上的兔兔（macOS / Windows 都可以養）：會走路、爬牆、站在 Dock／工作列上，
肚子餓會哭哭討布丁，每 45 分鐘提醒你喝水，每 25 分鐘變成巨兔提醒你休息，
上班時間陪你打電腦，下班時間帶頭慶祝狂奔。

> ⚠️ 僅供朋友私下使用。兔兔素材為 nagano《ちいかわ》角色的同人加工，
> **請不要公開散佈或上傳到公開平台**。引擎程式碼為 GPLv3 開源（見下方版權）。

---

## 下載

到 [Releases](https://github.com/yijiedai1108-source/tutu-desktop-pet/releases) 頁面下載對應平台的包：

| 平台 | 檔案 | 需求 |
|---|---|---|
| macOS | `tutu-desktop-pet.zip` | Apple Silicon（M1 以上）、macOS 15+ |
| Windows | `tutu-windows-x86_64.zip` | Windows 10 / 11（64 位元） |

## 安裝：macOS（三分鐘）

1. 解壓縮發佈包，會拿到兩個東西：
   - `Shijima-Qt.app` — 桌寵引擎（改造版）
   - `usagi-shimeji-v5.zip` — 兔兔素材包（**不要解壓縮**）
2. 打開 `Shijima-Qt.app`。macOS 會跳「無法打開」——先按「完成」（**不要**按「丟到垃圾桶」），
   再到 **系統設定 → 隱私權與安全性**，捲到最下面找到被阻擋的 Shijima-Qt，
   按 **「強制打開／仍要打開」** 並輸入密碼。
   （macOS 15 Sequoia 開始「右鍵 → 打開」已經不能繞過這個檢查，一定要走系統設定。）
   這是因為 app 沒有付費簽名，程式本身是開源可查的。
3. 把 `usagi-shimeji-v5.zip` **直接拖進** Shijima 的視窗完成匯入。
4. 在清單中**雙擊 Usagi**，兔兔就出現在桌面上了。
5. （建議）系統設定 → 隱私權與安全性 → **輔助使用** → 加入 Shijima-Qt 並打勾，
   兔兔才能站在你的視窗頂端。
6. （建議）第一次出現通知詢問時按「允許」，喝水/下班提醒才收得到。

## 安裝：Windows（三分鐘）

1. 把 zip **整個資料夾**解壓縮到任意位置（所有 DLL 要跟 `shijima-qt.exe` 放在一起）。
2. 雙擊 `shijima-qt.exe`。第一次開啟 SmartScreen 可能跳「Windows 已保護您的電腦」——
   點「**其他資訊**」→「**仍要執行**」。原因同上：沒有付費簽名，程式開源可查。
3. 把資料夾裡的 `usagi-shimeji.zip` **直接拖進** Shijima 的視窗完成匯入。
4. 在清單中**雙擊 Usagi**，兔兔就出現在桌面上了。
5. 提醒通知走系統匣的 🐰 圖示（原生 toast），不用額外設定；
   若沒看到通知，檢查「設定 → 系統 → 通知」有沒有整體關閉。

## 功能一覽

| 功能 | 說明 |
|---|---|
| 🚶 日常 | 亂走、發呆、坐下、睡覺、爬牆、爬天花板、站 Dock／工作列、拖曳丟擲 |
| 🍮 肚子餓 | 2 小時沒餵會哭哭，頭上冒出**想吃的食物**對話框（從已解鎖菜單隨機點菜，泡泡直接畫該道菜的圖案），點泡泡選菜餵食；**餵中牠想吃的那道**會冒 ❤️ 並額外加好感度 |
| 💧 喝水提醒 | 每 45 分鐘冒**水杯對話框**提醒你喝水，點擊選水量（100/250/500ml），杯中水位＝今日達成率，日標 2000ml 達標後當天不再吵 |
| 🍅 番茄鐘 | 每工作 25 分鐘，全部兔兔變成約 1/3 螢幕高的巨兔（高清素材，不會糊）提醒你休息，頭上冒隨機開場語的**倒數泡泡**（「休息囉～🐰 4:32」「ヤハ！休息時間！」…），5 分鐘後自動縮回重新計時；右鍵可「⏭ 跳過休息」或整個關掉（關掉後改回每 50 分鐘的久坐通知＋伸展操） |
| 💻 上班打電腦 | 平日上班時間（預設 09:00–18:00）偶爾拿出兔子牌筆電打字 |
| 🎉 下班儀式 | 到下班時間：通知＋對話框「下班了！」＋全場狂奔 20 秒 |
| ❤️ 好感度 | 餵食/喝水/每日見面加分，冷落哭哭扣分；5 級稱號（陌生兔兔→有點熟了→朋友→麻吉→家人），解鎖越高級的食物（布丁→飯糰→郎拉麵→草莓蛋糕→布丁阿拉摩德）；Lv3+ 會撒嬌、Lv4+ 偶爾追滑鼠找你玩、Lv5 每天第一次見面會迎接你 |
| ヤハ！ | 隨機冒出吶喊對話框（ヤハ！ウラ！…），好感度高會撒嬌 ❤️ |
| 🔊 叫聲 | 吶喊泡泡會配烏薩奇本聲（ヤハ／嗚啦~呀哈呀哈／哈咿呀哈），下班狂奔有開場吶喊；右鍵「🔊 兔兔叫聲」可整批靜音 |
| 🐇 增殖 | 兔兔偶爾會分裂或從螢幕邊拔出新兔兔（**上限 20 隻**，額滿自動停止，螢幕閒置也不怕爆量；手動生成不受限），右鍵 → Dismiss all but one 可清場；完全不想生：管理視窗 Settings → Enable multiplication 取消勾選 |

## 右鍵選單（點兔兔）

- 最上面三行是狀態：飢餓（哭哭時會寫牠想吃什麼）、今日飲水、好感度
- **餵食** / **喝水**：手動記錄（餵食選單依好感度等級解鎖菜色）
- **動作**：手動觸發任何動作（全中文清單，想看打電腦選「打電腦」、伸展選「伸展操」）
- **🔊 兔兔叫聲**：勾選開關（開會時取消勾選＝全部靜音，設定會記住）
- **🍅 番茄鐘休息（25/5）**：勾選開關（預設開）；休息中會多一項「⏭ 跳過休息」
- **Dismiss**：收掉兔兔

## 自訂設定

Windows 版開箱即是調校好的預設值（含移動速度），一般不需要調；
想改下班時間之類的進階設定找飼育員。macOS 用終端機指令（改完重開 app）：

```bash
# 下班時間（預設 18:00）
defaults write com.pixelomer.Shijima-Qt "workday.end" -string "18:30"

# 上班開始時間（預設 09:00，影響打電腦時段）
defaults write com.pixelomer.Shijima-Qt "workday.start" -string "09:30"

# 重設好感度 / 直接調分數（50=Lv2, 150=Lv3, 300=Lv4, 600=Lv5）
defaults write com.pixelomer.Shijima-Qt "affection.points" -int 0

# 繁殖上限（預設 20，0 = 不限；手動生成不受限）
defaults write com.pixelomer.Shijima-Qt "mascot.maxCount" -int 20

# 番茄鐘巨兔的大小（128px 的幾倍；未設＝約 1/3 螢幕高）
defaults write com.pixelomer.Shijima-Qt "pomodoro.giantScale" -float 4
```

想快速體驗全部功能（時間加速 60 倍，2 小時變 2 分鐘）：

```bash
# macOS（終端機）
SHIJIMA_HUNGER_SCALE=60 /Applications/Shijima-Qt.app/Contents/MacOS/shijima-qt
```

```bat
:: Windows（在解壓縮資料夾開 cmd）
set "SHIJIMA_HUNGER_SCALE=60" && shijima-qt.exe
```

## 開機自動啟動（可選）

**macOS**：把 `launchagent/com.pixelomer.ShijimaQt.plist` 裡的路徑改成你的 app 實際位置，然後：

```bash
cp launchagent/com.pixelomer.ShijimaQt.plist ~/Library/LaunchAgents/
```

反悔就刪掉那個檔案。

**Windows**：按 `Win + R` 輸入 `shell:startup` 打開啟動資料夾，
把 `shijima-qt.exe` 的**捷徑**（右鍵 → 建立捷徑）丟進去。反悔就刪掉捷徑。

## FAQ

- **兔兔越生越多？** 這是 Shimeji 傳統藝能（分裂＋拔蘿蔔），右鍵 → Dismiss all but one。
- **沒聽到叫聲？** 檢查右鍵選單「🔊 兔兔叫聲」有勾；叫聲跟吶喊泡泡一起出現（十幾分鐘一次，好感度越高越常喊），想立刻聽就右鍵 → 🎉 下班狂奔。**先前匯入過任何舊素材包（含 v4）的人請重新匯入 v5**（v4 之前沒聲音；v4 的行為設定是未調校版）。
- **清單裡有個叫「img」的？／兔兔放著會定住不動？** 都是舊素材包匯入 bug 的症狀：在管理視窗刪掉「img」，重新匯入 `usagi-shimeji-v5.zip`，改用 **Usagi** 就都好了。
- **通知沒出現？（macOS）** 系統設定 → 通知 → 找「Script Editor／工序指令編寫程式」→ 允許。
- **通知沒出現？（Windows）** 設定 → 系統 → 通知 → 確認通知整體開啟；toast 由系統匣的 🐰 圖示發出。
- **兔兔站不上視窗？（macOS）** 檢查輔助使用權限（見 macOS 安裝第 5 步）。Windows 不需要額外權限。
- **Intel Mac 能玩嗎？** 現成包只編了 Apple Silicon 版，需要的話可以照 `patches/README.md` 自己編。
- **Windows 版怪怪的？** Windows 版較新、實測較少（通知、速度手感尤其想聽回饋），遇到問題回報給飼育員。

## 版權與致謝

- 引擎基於 [pixelomer/Shijima-Qt](https://github.com/pixelomer/Shijima-Qt)（GPLv3）改造，
  修改內容與建置方法見 `patches/`（Windows 版由 GitHub Actions 自動建置，見 `.github/workflows/`）；
  本 fork 同樣以 GPLv3 授權，原始碼都在這個 repo。
- 兔兔素材源自網路同人 Shimeji 素材（原角色 © nagano／《ちいかわ》），
  經本專案加工（表情、食物、動作幀）。**僅限私人使用，請勿公開散佈。**
