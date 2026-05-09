# keyboards

自作キーボードのファームウェアを管理するリポジトリです。
GitHub Actions でビルドを自動化しており、ローカルに環境を構築せずに `.uf2` ファイルを生成できます。

## キーボード一覧

| ディレクトリ | キーボード | 詳細 |
|---|---|---|
| [`corne_v4/`](corne_v4/README.md) | Corne V4 (Rev 4.1 Standard) | via / vial |

## ディレクトリ構成

```
keyboards/                     ← このリポジトリ
  <キーボード名>/
    README.md                  ← キーボード固有の説明
    keymaps/
      <キーマップ名>/           ← キーレイアウト定義
  .github/
    workflows/
      build.yml                ← GitHub Actions ワークフロー
```

## ファームウェアのビルド方法

### GitHub Actions で自動ビルド（推奨）

`main` ブランチへの push または PR 作成時に自動でビルドが走ります。

1. GitHub リポジトリの **Actions** タブを開く
2. 最新のワークフロー実行を選択
3. **Artifacts** セクションから `<キーボード名>-firmware` をダウンロード
4. ZIP を展開すると `.uf2` ファイルが取得できる

手動でビルドを実行したい場合は、Actions タブ → **Build Firmware** → **Run workflow** から起動できます。

### ファームウェアの書き込み

1. キーボードをリセットボタン2回押しでブートローダーモードにする
2. USB ドライブとしてマウントされるのを確認
3. `.uf2` ファイルをドライブにコピーする
4. 自動的に再起動してファームウェアが適用される

## キーボードを追加する

### 1. キーマップファイルを追加

```
<新しいキーボード名>/
  README.md
  keymaps/
    <キーマップ名>/
      keymap.c
      config.h
      rules.mk
```

既存の `corne_v4/` を参考にしてください。

### 2. ワークフローに追加

`.github/workflows/build.yml` の `matrix.include` にエントリを追加します。

```yaml
matrix:
  include:
    - name: <キーボード名>
      firmware_repo: vial-kb/vial-qmk
      qmk_keyboard: <QMK でのキーボードパス>
      qmk_keymap: vial

    - name: <キーボード名>
      firmware_repo: qmk/qmk_firmware
      qmk_keyboard: <QMK でのキーボードパス>
      qmk_keymap: via
```

`qmk_keyboard` の値は各ファームウェアリポジトリの `keyboards/` 以下のパスです。
例: `crkbd/rev4_1/standard`, `lily58/rev1`, `helix/rev2`

## 参考リンク

- [vial-kb/vial-qmk](https://github.com/vial-kb/vial-qmk) — Vial 対応ファームウェアのベースリポジトリ
- [qmk/qmk_firmware](https://github.com/qmk/qmk_firmware) — QMK ファームウェア本体
