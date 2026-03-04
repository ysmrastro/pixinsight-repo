---
layout: single
title: "Manual Image Solver"
---

[English](en/) \| [GitHub リポジトリ](https://github.com/ysmr3104/manual-image-solver)

手動プレートソルブツール。画像上の星をユーザーが手動で同定し、TAN（gnomonic）投影の WCS（World Coordinate System）を算出して画像に適用します。

## 概要

astrometry.net や PixInsight の ImageSolver による自動プレートソルブが失敗する画像に対し、手動で星を同定して WCS を取得するためのツールです。

**PixInsight 完結**: Python 等の外部依存なし。PixInsight の PJSR ネイティブ Dialog 内で、画像表示から星選択、WCS フィッティング、適用まで全ての操作が完結します。

![メインダイアログ - 星登録済み]({{ site.baseurl }}/assets/images/manual-image-solver/03_stars_registered.jpg)

## 主な機能

- **直感的な操作**: クリックで星選択、ドラッグでパン（モード切替不要）
- **ストレッチ切替**: None / Linked / Unlinked の 3 モードをワンクリックで切替
- **19 段階ズーム**: マウスホイール（マウス位置中心）、Fit / 1:1 ボタン、+/- ボタン
- **セッション復元**: 前回の星ペアデータを自動保存し、次回起動時に復元可能
- **Export / Import**: 星ペアデータを JSON ファイルに書き出し・読み込み
- **Sesame 検索**: 天体名から CDS Sesame データベースで RA/DEC を自動取得
- **セントロイド計算**: クリック位置から輝度重心法で星の中心に自動スナップ

## インストール

### リポジトリからインストール（推奨）

1. PixInsight のメニューから **Resources > Updates > Manage Repositories** を開く
2. **Add** をクリックし、以下の URL を入力:
   ```
   https://raw.githubusercontent.com/ysmr3104/manual-image-solver/main/repository/
   ```
3. **OK** で閉じ、**Resources > Updates > Check for Updates** を実行
4. PixInsight を再起動

### 手動インストール

1. このリポジトリを clone またはダウンロード
2. PixInsight で **Script > Feature Scripts...** を開く
3. **Add** → `manual-image-solver/javascript/` ディレクトリを選択
4. **Done** で閉じる → **Script > Utilities > ManualImageSolver** がメニューに追加される

Python や外部パッケージのインストールは不要です。

## 使い方

### 1. スクリプトを起動する

PixInsight で対象画像を開き、**Script > Utilities > ManualImageSolver** を実行します。

ダイアログが開き、ストレッチ済みの画像が表示されます。

![メインダイアログ - 初期状態]({{ site.baseurl }}/assets/images/manual-image-solver/01_gui_initial.jpg)

前回のセッションデータが残っている場合は、復元するかどうかの確認ダイアログが表示されます。

<img src="{{ site.baseurl }}/assets/images/manual-image-solver/08_restore_dialog.jpg" alt="セッション復元ダイアログ" width="480">

### 2. 星を登録する

画像上の星を**クリック**すると、セントロイド計算で星の中心に自動スナップし、座標入力ダイアログが開きます。

<img src="{{ site.baseurl }}/assets/images/manual-image-solver/02_star_dialog.jpg" alt="星座標入力ダイアログ" width="480">

**天体名**を入力して **Search** をクリックすると、CDS Sesame データベースから RA/DEC が自動入力されます。RA/DEC を直接入力することもできます。

#### 座標入力フォーマット

| 項目 | フォーマット例 |
|---|---|
| RA（HMS） | `05 14 32.27` / `05:14:32.27` |
| RA（度数） | `78.634` |
| DEC（DMS） | `+07 24 25.4` / `-08:12:05.9` |
| DEC（度数） | `7.407` / `-8.202` |

### 3. Solve を実行する

4 星以上を登録したら **Solve** ボタンをクリックします。WCS フィッティングが実行され、各星の残差が表示されます。

### 4. WCS を適用する

**Apply to Image** をクリックすると、WCS がアクティブ画像に直接適用されます。

Process Console にはフィット結果の詳細（各星の残差、画像四隅の座標、FOV、回転角度など）が表示されます。

![Process Console 出力]({{ site.baseurl }}/assets/images/manual-image-solver/06_process_console.jpg)

### 5. 結果を確認する

WCS 適用後、PixInsight の **AnnotateImage** で星座や天体のアノテーションを重ねて確認できます。

![AnnotateImage による確認]({{ site.baseurl }}/assets/images/manual-image-solver/07_annotated_image.jpg)

### 操作ヒント

- **星選択**: 画像上をクリック（セントロイドで自動スナップ）
- **パン**: 左ボタンドラッグ、または中ボタンドラッグ
- **ズーム**: マウスホイール（マウスカーソル位置を中心にズーム）
- **ズームボタン**: Fit（ウィンドウにフィット）、1:1（等倍）、+（拡大）、-（縮小）
- **ストレッチ切替**: ツールバーの STF: None / Linked / Unlinked ボタン
- **星の編集**: テーブル行をダブルクリック、または選択して **Edit...**
- **星の削除**: 選択して **Remove**
- **Export / Import**: 星ペアデータを JSON ファイルに保存・読み込み

## ライセンス

[MIT License](https://github.com/ysmr3104/manual-image-solver/blob/main/LICENSE)
