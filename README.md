# zmk-config-AroundFortyLB (zmk-v0.4_inertial-scroll)

Around Forty LB の ZMK v0.4（Zephyr 4.1）対応＋**慣性スクロール・制御Behavior** 搭載版ファームウェアです。

本ブランチは、**ZMK v0.4 (Zephyr 4.1)** 環境において `razilyis/zmk-pmw3610-driver`（`Dev-v0.4_inertial-scroll` ブランチ）を採用し、**badjeff 本家準拠の超高精度・滑らかなカーソル追従性**と、なめらかな**慣性スクロール・各種トグルBehavior**を利用可能にしたブランチです。
Around Forty LB は **左手側（L）が Central かつトラックボール搭載**、**右手側（R）が Peripheral** の構成となっています。

---

## 主な実装内容・特徴

### 🟢 最高精度の 1:1 カーソル追従性（通常ポインティング時）
- 通常のカーソル操作時は、余分なフィルタや遅延処理を挟まず、badjeff 本家（`zmk-v0.4`）とまったく同一のダイレクトな高速サンプリング（ゼロ遅延・完全な滑らかさ）で動作します。

### 🟢 慣性スクロール (ZMK v0.4 対応)
- **自然な慣性スクロール**: レイヤー 7 でのスクロール操作時に、指を離した後も心地よい減速を伴う慣性スクロールを実行
- **ジェスチャー速度正規化**: フリックの勢いを正確に維持しつつ、過剰な暴走を防止
- **制御Behaviorのサポート (Keymap Editor 対応)**:
  - `&pmw3610_inertia_toggle`: 慣性スクロールの ON / OFF 切り替え
  - `&pmw3610_scroll_direction_toggle`: 縦スクロール方向の正転 / 反転切り替え
  - `&pmw3610_horizontal_scroll_direction_toggle`: 横スクロール方向の正転 / 反転切り替え

### 🟢 ZMK v0.4 (Zephyr 4.1) への移行
- **最新 Zephyr 4.1 対応**: ZMK main（Zephyr 4.1 系統）を pin し、新規格に適合
- **新ボード定義形式への対応**: ボード指定を `xiao_ble//zmk`、インターコネクト ID を `seeed_xiao` に更新
- **Devicetree での NFC ピン GPIO 化**: Zephyr 4.1 での Kconfig 廃止に伴い、P0.09 / P0.10 の GPIO 再利用指定を DTS（`&uicr`）へ移行
- **外部モジュールの Zephyr 4.1 追従**:
  - `razilyis/zmk-pmw3610-driver` (`Dev-v0.4_inertial-scroll`): Zephyr 4.1 上流との衝突回避のため `pixart,pmw3610-alt` / `CONFIG_PMW3610_ALT_*` に完全追従
  - `caksoylar/zmk-rgbled-widget`: Zephyr 4.1 対応版へ更新

### 🟢 トラックボールおよび BLE 通信の最適化
- **トラックボール応答性・復帰速度の改善**: 報告間隔（15ms）の同期および復帰遅延チューニング
- **BLE スタックメモリ・バッファ最適化**: `CONFIG_BT_HCI_TX_STACK_SIZE_WITH_PROMPT=y` の適用など左右のスタック設定を最適化
- **左右の役割分担の最適化**:
  - Central 側（左手 L）: トラックボール制御、BLE ホスト接続、Studio 通信、バッテリープロキシ
  - Peripheral 側（右手 R）: キー入力スキャン、子機 BLE 通信、不要機能の無効化

### 🟢 ZMK Studio 対応
- `studio-rpc-usb-uart` スニペットを有効化し、USB 接続時のリアルタイムキーマップ編集に対応

### 🟢 全角半角の切り替えマクロ
- マクロにより、1 つのキーで日本語入力の全角/半角トグル切り替えが可能

---

## 保留事項
- 🟡 **Prospector Scanner**: Bluetooth 接続の安定性を最優先するため、本ブランチでも非対応としています

---

## ご利用ガイド

キーボードの使い方や設定の詳細は以下をご参照ください。

- [ご利用ガイド (note)](https://note.com/razily/n/n0b3c5ff58d92)
