# Corne V4 (Rev 4.1 Standard)

Corne V4 のファームウェア管理ディレクトリです。
via と vial の2種類のキーマップをビルドします。

## キーマップ一覧

| キーマップ | ファームウェアベース | ビルドターゲット | 用途 |
|---|---|---|---|
| `vial` | vial-kb/vial-qmk | `crkbd/rev4_1/standard:vial` | メインで使用するキーマップ。Vial GUI でリアルタイム編集可能 |
| `via` | qmk/qmk_firmware | `crkbd/rev4_1/standard:via` | VIA 対応ファームウェア |

## ディレクトリ構成

```
corne_v4/
  keymaps/
    vial/
      keymap.c       ← キーレイアウト定義
      config.h       ← Vial UID・アンロックコンボ設定
      rules.mk       ← 有効にする機能の設定
      vial.json      ← Vial GUI 用レイアウト定義
    via/
      config.h       ← ハードウェアピン・RP2040 設定
      mcuconf.h      ← I2C 有効化（MCU レベル）
      post_config.h  ← OLED フォントパス設定
```

### via/ にハードウェア設定が入っている理由

`via/` 以下のファイルは「via キーマップ固有の設定」ではなく、**qmk/qmk_firmware でビルドする際に必要なハードウェア差分パッチ**です。

vial-qmk リポジトリは Corne V4 のハードウェア設定（ピン配置・I2C・RP2040 設定）をすでに内包しているため、vial ビルドではこれらのファイルは不要です。一方 qmk/qmk_firmware はこれらを持っていないため、via キーマップのディレクトリに上書き設定として配置しています。

また vial ビルドは OLED を使用しないため、`post_config.h`（OLEDフォントパス）も via 側のみに存在します。

## キーマップのカスタマイズ

### Vial GUI で変更する場合

[Vial](https://get.vial.today/) をインストールしてキーボードを接続するだけで編集できます。
この場合、リポジトリのファイルを変更する必要はありません。

### `keymap.c` を直接編集する場合

`keymaps/vial/keymap.c` を変更して `main` に push すると、ビルドが自動で走り新しい `.uf2` が生成されます。

## 参考リンク

- [foostan/kbd_firmware](https://github.com/foostan/kbd_firmware) — Corne 公式キーマップ
- [vial-kb/vial-qmk](https://github.com/vial-kb/vial-qmk) — vial-qmk リポジトリ
- [Vial 公式サイト](https://get.vial.today/) — Vial GUI アプリ
