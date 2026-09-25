# torabo-tsuki LP XS カスタムファーム

公式の`zmk-keyboard-torabo-tsuki-lp`を土台に、roBaから移した42キー用キーマップを`config/keymap.keymap`へ実装した。

## 初版のトラックボール設定

- PAW3222 CPI：ドライバ既定値（`res-cpi`を指定しない）
- 通常カーソル倍率：1:1
- オートマウス：MOUSEレイヤー（layer 4）、解除時間700ms
- クリック位置：J＝左、K＝中、L＝右
- SCROLLレイヤー（layer 5）：XYを縦・横スクロールへ変換
- スクロール倍率：1:1
- SCROLLレイヤーの処理ではオートマウスを発火させない
- オートマウス中にJ／K／Lを押してもMOUSEレイヤーを即時解除しない

CPIと倍率は同時に変更せず、まず既定CPI・1:1を実機基準とする。速度が一様に不足／過剰なら`res-cpi`、細かな調整は`zip_xy_scaler`を変更する。スクロールだけは`zip_scroll_scaler`で独立調整する。

## 実装済み

- DEFAULT／FUNCTION／NUM／ARROW／MOUSE／SCROLL／BT
- Z長押しShift、I長押しSCROLL
- Space長押しNUM、無変換長押しARROW
- Backspace長押しFUNCTION、CapsLock長押しBT

### 右親指の実位置

2026-08-16、positions 59〜62を数字1〜4へ置換した診断ファームで実機確認した結果、Enter側はposition 59、Backspace側はposition 60だった。右トラックボール構成で欠ける位置はposition 61 / SW68とposition 62 / SW67であり、主レイヤーはposition 59をEnter、position 60をBackspace長押しFUNCTIONとしている。

同日の実機確認で、SCROLL層は横方向が意図どおり、縦方向だけ逆だった。カーソル層はXY反転のまま維持し、SCROLL層のみX反転へ変更して縦方向を反転させた。

右親指位置の確定後、ARROW層に旧位置対応が残っていたため、LWinをposition 59、LAltをposition 60へ移動した。エディタ案およびSVGは既にこの実位置順だった。

Backspaceのキーリピートを妨げないよう、FUNCTION層への長押し遷移をBackspaceからEnterへ移した。DEFAULT層はEnter長押しFUNCTION、Backspace単機能とし、FUNCTION層内に置いていた補助Backspaceは削除した。
- S＋D＝Tab、D＋F＝Shift＋Tab
- BT0〜BT4、BT clear、BT clear all、bootloader

## ビルド対象

右トラックボール構成では、`build.yaml`にある次の2成果物を使用する。

- `torabo_tsuki_lp_right_central`
- `torabo_tsuki_lp_left_peripheral`

このPCには2026-08-16時点で`west`、CMake、Ninja、Dockerがないため、ローカルビルドは未実施。リポジトリをGitHubへ置き、既存のGitHub Actionsを実行して構文・依存関係を検証する。

## 初回ビルド

2026-08-16、GitHub Actions run 31946798812で全ビルドに成功。成果物は`artifacts/20260816-initial/`へ取得した。右トラックボール構成で書き込むファイルは次の2個。

- `artifacts/20260816-initial/torabo_tsuki_lp_right_central.uf2`
- `artifacts/20260816-initial/torabo_tsuki_lp_left_peripheral.uf2`

## 現在の保存済み最新版（2026-08-16 23:39）

初版以降の親指位置、スクロール方向、ARROW層、Enter長押しFUNCTIONの修正を含む最終ビルドは`artifacts/20260816-enter-function/`に保存した。右トラックボール構成へ書き込むのは次の2ファイル。`double_ball`、`left_central`、`right_peripheral`、`settings_reset`は通常の左右構成用ではない。

- 右側：`artifacts/20260816-enter-function/torabo_tsuki_lp_right_central.uf2`
- 左側：`artifacts/20260816-enter-function/torabo_tsuki_lp_left_peripheral.uf2`

## 2台目のBluetooth表示名

`build.yaml`に、従来のビルドと併存する`torabo-tsuki2`専用の右Central／左Peripheralビルドを追加した。2台目だけに書き込む。UF2は未生成であり、ビルド成功と実機反映は未確認。

ZMKはBluetooth名を保存設定に保持するため、名称変更後は2台目の左右両側の設定を初期化し、ホスト側で旧ペアリングを削除して再ペアリングする必要がある。初号機の設定は消さない。
