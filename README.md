# mogitate-fruits-test2

## 概要　
このリポジトリは、確認テスト第2回の課題として作成した「果物の商品を管理するアプリケーション」です。
Laravel と Docker を使用して、ローカル環境で動作する商品管理機能を構築しています。 
マイグレーションとシーディングにより、初期データ（商品・季節・カテゴリ）を投入できるようになっています。

---

## 🛠️ 環境構築手順

### 1. リポジトリの設定
このプロジェクトのベースとなるコードを取得するために、GitHubからリポジトリをクローンします。

### 2. Docker の設定
ローカル環境に必要なサービス（PHP, MySQLなど）をDockerで構築・起動します。

以下のコマンドでDocker環境を構築・起動しました：
```bash
docker-compose up -d --build
```

### 3. Laravel のパッケージのインストール
Laravelの動作に必要な依存パッケージをインストールします。
```bash
docker-compose exec app bash
composer install
```

### 4. .env ファイルの作成
Laravelの環境設定を行うために、.envファイルを作成し、アプリケーションキーを生成します。
```bash
cp .env.example .env
php artisan key:generate
```

### 5. View ファイルの作成（前回の内容なので、提出前に修正します）
ユーザーが入力するお問い合わせフォーム画面や関連画面を作成しました。  
主なファイル:
- `resources/views/inquiry/form.blade.php` （お問い合わせフォーム）
- `resources/views/inquiry/confirm.blade.php` （確認画面）
- `resources/views/inquiry/thanks.blade.php` （送信完了画面）
- `resources/views/layouts/partials/header.blade.php` / `footer.blade.php` （共通レイアウト）
- `resources/views/auth/login.blade.php` / `register.blade.php` （認証関連）

### 6. CSS ファイルの作成（前回の内容なので、提出前に修正します）
フォームや各画面のデザインを整えるためのスタイルを作成しました。  
主なファイル:
- `public/css/form.css` （お問い合わせフォーム用）
- `public/css/confirm.css` （確認画面用）
- `public/css/thanks.css` （完了画面用）
- `public/css/login.css` / `register.css` （認証画面用）
- `public/css/admin.css` （管理画面用）
- `public/css/sanitize.css` （リセット用）
- `public/css/style.css` （共通スタイル）

## 🛠 使用技術（この例で使われている環境）
- PHP 8.2
- Laravel 10.0
- MySQL 8.0
- Docker (nginx, php, mysql, phpmyadmin)

## 🗂 ER図（このプロジェクトのデータ構造）（前回の内容なので、提出前に修正します）
このアプリケーションのデータ構造を視覚的に把握するため、以下にER図を掲載しています。

この図では、`contacts` テーブルが `categories` テーブルに属する「1対多」のリレーション（has_many）を矢印で表現しています。
各テーブルは表形式（右揃え）で構成されており、主キー（PK）・外部キー（FK）の役割が明示されています。  
また、`gender` カラムは仕様書に従い **tinyint 型（1: 男性、2: 女性）** として保存されます。

![ER図](assets/contact-form-er-v2.png)

※ 補足：
1. 図は draw.io（diagrams.net）にて作成し、PNG形式で保存しています。
2. 元データは `src/contact-form-er-v2.drawio` にて編集可能です。
3. PNGファイルは assets/contact-form-er-v2.png に保存されています。
   → READMEではこの画像を参照しています。
4. 編集には [draw.io（diagrams.net）](https://app.diagrams.net/) を使用してください。  
　 ローカルアプリまたはブラウザ版のどちらでも編集可能です。
5. ER図の更新手順：drawioで編集 → PNG再出力 → assetsに上書き保存 → README確認
   ※GitHub上で画像が更新されない場合は、Shift+再読み込み（Ctrl+Shift+R）などでキャッシュを強制クリアしてください。

### データ仕様（要点）

- products テーブル  
  - name: string 型（商品名）  
  - price: integer 型（価格）  
  - description: text 型（説明文）  

- seasons テーブル  
  - name: string 型（季節名：春・夏・秋・冬）  

- 中間テーブル product_season  
  - product_id: 外部キー（products.id）  
  - season_id: 外部キー（seasons.id）  

- Seeder では、商品データを投入し、対応する季節データと関連付けて登録  
  - 例:  
    ```php
    $seasonIds = Season::whereIn('name', $seasonNames)->pluck('id');
    $product->seasons()->attach($seasonIds);
    ```

### ダミーデータの作成

- seasons テーブル: Seeder を使用して以下 4 件を作成  
  1. 春  
  2. 夏  
  3. 秋  
  4. 冬  

- products テーブル: Seeder を使用して複数の商品データを作成  
  - name, price, description を持つ商品を投入  
  - 各商品は複数の季節と関連付けられる  

- 中間テーブル product_season:  
  - ProductSeeder 内で `$product->seasons()->attach($seasonIds);` を使用し、  
    商品と季節の多対多関係を登録
 
## 🌐 ローカル環境での確認用URL
- アプリケーション: [http://localhost/](http://localhost/)
- phpMyAdmin: [http://localhost:8080/](http://localhost:8080/)

## 実装状況
- [x] Docker 環境構築 (nginx, php, mysql, phpmyadmin)
- [x] Laravel プロジェクト作成
- [x] モデル・マイグレーション作成（products, seasons, 中間テーブル）
- [x] Seeder による初期データ投入（商品・季節・関連付け）

## 今後の実装予定
- [ ] Blade による画面作成（商品一覧・詳細・登録フォーム）
- [ ] バリデーション実装（商品登録フォームの入力チェック）
- [ ] エラーメッセージの表示整備（ユーザーにわかりやすいフィードバック）
- [ ] デザイン調整（レイアウトやスタイルの改善）
- [ ] テストコードの追加（ユニットテスト・Featureテスト）

## 既知の問題（前回の内容なので、提出前に修正します）
- `php artisan migrate:fresh --seed` 実行時に **migrations テーブル作成でエラー**が発生  
- データベース自体は `utf8mb4` 設定済みだが、テーブル作成時にエンジン・照合順序でエラーが出る  
- 時間の都合で完全解決には至らず、現状のまま提出  

## 提出にあたって（前回の内容なので、提出前に修正します）
- main ブランチにコミット済み  
- `.env` や `vendor/` ディレクトリはセキュリティ・再現性の観点からコミットしていません  

## 動作確認（前回の内容なので、提出前に修正します）
- アプリケーション: http://localhost/  
- phpMyAdmin: http://localhost:8080/  

## ライセンス
このリポジトリは学習・確認テスト用に作成したものであり、商用利用は想定していません。
