# dbt DuckDB Practice

このプロジェクトは、dbt (data build tool) と DuckDB を組み合わせてデータ変換の練習を行うためのリポジトリです。

## 1. プロジェクト構成

```text
/
├── dbt_project/          # dbt プロジェクトのメインディレクトリ
│   ├── dbt_project.yml   # プロジェクト設定
│   ├── dev.duckdb        # DuckDB データベースファイル
│   ├── models/           # SQLモデル
│   ├── seeds/            # CSVデータ
│   └── ...
├── venv/                 # Python 仮想環境
├── README.md             # 本ファイル
└── SETUP_LOG.md          # セットアップ記録
```

## 2. セットアップ手順

### ステップ1：環境の準備
1. **Python仮想環境の有効化**
   ```bash
   source venv/bin/activate  # Windowsの場合: .\venv\Scripts\activate
   ```

2. **dbt-duckdb のインストール**
   ```bash
   pip install dbt-duckdb
   ```

### ステップ2：接続設定 (profiles.yml)
dbtは `~/.dbt/profiles.yml` を参照します。DuckDBを使用するために以下の設定を追加または作成してください。

```yaml
dbt_duckdb_practice:
  outputs:
    dev:
      type: duckdb
      path: /Users/ymto/Documents/git/dbt-duckdb-practice/dbt_project/dev.duckdb
      threads: 4
  target: dev
```
※ `path` は実際の環境に合わせて絶対パスで指定することを推奨します。

### ステップ3：接続確認
```bash
cd dbt_project
dbt debug
```

## 3. 基本的なコマンド
- `dbt seed`: `seeds/` 内のCSVをDuckDBにロード
- `dbt run`: モデルの実行（テーブル/ビューの作成）
- `dbt test`: テストの実行
- `dbt docs generate`: ドキュメント生成
- `dbt docs serve`: ドキュメントの表示

## 4. 学習ロードマップ（Todo）
- [ ] `dbt seed` でサンプルデータをロード
- [ ] `sources.yml` の定義
- [ ] Staging層 / Mart層の構築
- [ ] DuckDB固有の機能（S3/Parquet読み込みなど）の試行

## 5. 参考リソース
- [dbt-duckdb Adapter Documentation](https://github.com/jwills/dbt-duckdb)
- [dbt Documentation](https://docs.getdbt.com/)