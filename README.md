# Laravel Docker Environment

PHP 8.3 + nginx + MySQL を使用したLaravelプロジェクト用のDocker環境です。

## 構成

- **PHP**: 8.3-fpm (Composer 2.7含む)
- **Web Server**: nginx 1.25-alpine  
- **Database**: MySQL 8.0
- **Port**: 60 (HTTP), 3360 (MySQL)

## 使い方

### 1. 環境変数の設定
```bash
cp .env.example .env
# 必要に応じて.envを編集
```

### 2. 開発環境の起動
```bash
docker-compose up -d
```

### 3. Laravelプロジェクトの配置
`src/` ディレクトリにLaravelプロジェクトを配置してください。

### 4. アクセス
- アプリケーション: http://localhost:60
- データベース: localhost:3360

## よく使うコマンド

```bash
# コンテナの状況確認
docker-compose ps

# ログの確認
docker-compose logs -f

# PHPコンテナに入る
docker-compose exec app bash

# Composer実行
docker-compose exec app composer install

# Artisanコマンド実行
docker-compose exec app php artisan migrate

# 環境の停止
docker-compose down

# 環境の完全削除（データベースも削除）
docker-compose down -v
```

## 本番環境

本番環境では以下を想定：
- Aurora MySQL（ローカルのMySQLは使用しない）
- CloudFront + ACM（SSL証明書の自動管理）

```bash
# 本番環境での起動
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

## ディレクトリ構成

```
.
├── infra/
│   ├── mysql/          # MySQL設定
│   ├── nginx/          # nginx設定
│   └── php/            # PHP設定
├── src/                # Laravelプロジェクトを配置
├── docker-compose.yml  # 開発環境設定
├── docker-compose.prod.yml # 本番環境設定
└── .env.example        # 環境変数テンプレート
```

## 注意事項

- `src/` ディレクトリは空の状態です
- 本番環境ではCloudFrontとACMを使用することを推奨
- データベースの永続化は `db-store` ボリュームで行われます