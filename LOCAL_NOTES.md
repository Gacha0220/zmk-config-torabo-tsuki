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
- S＋D＝Tab、D＋F＝Shift＋Tab
- BT0〜BT4、BT clear、BT clear all、bootloader

## ビルド対象

右トラックボール構成では、`build.yaml`にある次の2成果物を使用する。

- `torabo_tsuki_lp_right_central`
- `torabo_tsuki_lp_left_peripheral`

このPCには2026-08-16時点で`west`、CMake、Ninja、Dockerがないため、ローカルビルドは未実施。リポジトリをGitHubへ置き、既存のGitHub Actionsを実行して構文・依存関係を検証する。
