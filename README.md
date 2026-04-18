## セットアップ手順

### ステップ1：AWS環境の準備
1. **Redshift Serverless のセットアップ**
   - AWSコンソールから「Redshift Serverless」を選択。
   - ワークグループとネームスペースを作成。
   - 外部から接続可能なように「ネットワークとセキュリティ」設定で「パブリックアクセス」をオンにする。
2. **IAMユーザーの作成**
   - 接続用のIAMユーザーを作成し、`AmazonRedshiftFullAccess` 権限を付与。
   - プログラムによるアクセス（Access Key / Secret Key）を取得。

### ステップ2：ローカル環境の構築
1. **Python仮想環境の作成と有効化**
   ```bash
   python -m venv venv
   source venv/bin/activate  # Windowsの場合: .\venv\Scripts\activate
   ```
2. **dbt-redshift のインストール**
   ```bash
   pip install --upgrade pip
   pip install dbt-redshift
   ```

### ステップ3：接続設定 (profiles.yml)
dbtはデフォルトで `~/.dbt/profiles.yml` を参照します。このファイルを作成し、以下の内容を記述します。
※ `dbt_project.yml` 内の `profile:` 名（デフォルトはプロジェクト名）と一致させる必要があります。

```yaml
dbt_redshift_practice:
  outputs:
    dev:
      type: redshift
      method: database
      host: [Redshiftのワークグループエンドポイント]
      user: [設定したユーザー名]
      password: [設定したパスワード]
      port: 5439
      dbname: dev
      schema: public  # 開発用スキーマ
      threads: 4
  target: dev
```

### ステップ4：dbtの初期化と接続確認
```bash
# プロジェクトディレクトリ内へ移動（dbt init済みの場合）
cd dbt_project

# 接続テストの実行
dbt debug
```

## 5. 基本的なコマンド
- `dbt seed`: `seeds/` ディレクトリ内のCSVをRedshiftにテーブルとしてロード
- `dbt run`: `models/` 内のSQLを実行してテーブル/ビューを作成
- `dbt test`: `schema.yml` に定義したテストを実行
- `dbt docs generate`: ドキュメントを生成
- `dbt docs serve`: 生成したドキュメントをローカルブラウザで表示

## 6. 学習ロードマップ（Todo）
- [ ] `dbt seed` で初期データをロードする
- [ ] `sources.yml` を定義してソースデータを管理する
- [ ] **Staging層**: ソースデータの型変換やリネームを実施
- [ ] **Mart層**: `ref` 関数を用いてテーブル間を結合し、分析用モデルを作成
- [ ] Generic Tests (unique, not_null) の実装
- [ ] GitHub Actions を用いた CI（Pull Request時の `dbt compile`）の構築

## 7. 参考リソース
- [dbt Documentation](https://docs.getdbt.com/)
- [dbt-redshift setup guide](https://docs.getdbt.com/docs/core/connect-data-platform/redshift-setup)