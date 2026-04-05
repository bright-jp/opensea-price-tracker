# OpenSea Price Tracker

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.jp)
[![OpenSea Price Tracker](https://img.shields.io/badge/OpenSea%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.jp/products/insights/price-tracker/opensea)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.jp/products/insights/price-tracker/opensea)

世界最大のNFTマーケットプレイスであるOpenSeaの価格をリアルタイムで追跡します。始める方法は2つあります。**フルマネージド**のインテリジェンスプラットフォーム、またはBright DataのAI Scraper Builderで構築する**カスタムスクレイパー**です。

---

## Option 1: Bright Insights - AI搭載価格トラッキング（推奨）

**[Bright Insights](https://brightdata.jp/products/insights/price-tracker/opensea)** は、Bright Dataのフルマネージドなリテールインテリジェンスプラットフォームです。スクレイパーを構築する必要も、インフラを保守する必要もありません。構造化され、分析可能な価格データが、ダッシュボード、データフィード、またはBIツールにそのまま配信されます。

**チームがBright Insightsを選ぶ理由:**
- 🚀 **セットアップ不要** - すぐに使えるダッシュボードとデータフィードで数分で本番稼働
- 🤖 **AI搭載のレコメンデーション** - 会話型AIアシスタントが数百万のデータポイントを即座に実用的なインサイトへ変換
- ⚡ **リアルタイム監視** - 1時間ごとから日次までの更新頻度と即時アラート（email、Slack、webhook）
- 🌍 **無制限のスケール** - あらゆるWebサイト、あらゆる地域、あらゆる更新頻度に対応
- 🔗 **プラグアンドプレイの連携** - AWS、GCP、Databricks、Snowflakeなど
- 🛡️ **フルマネージド** - スキーマ変更、サイト更新、データ品質をBright Dataが自動で処理

**主なユースケース:**
- ✅ すべての製品カテゴリにわたって**OpenSeaの価格を監視**
- ✅ **在庫レベルと可用性を追跡**し、リアルタイムで把握
- ✅ 注目している商品に対して**価格アラートを設定**
- ✅ MAPポリシー準拠を監視し、価格違反を検出
- ✅ 競合のプロモーションと販促動向を追跡
- ✅ クリーンで正規化されたデータを動的価格設定アルゴリズムやAIモデルに直接投入

> **月額$250から - [最適なお見積もりを取得 →](https://brightdata.jp/products/insights/price-tracker/opensea)**

---

## Option 2: 独自のOpenSea Scraperを構築

事前構築済みのOpenSea scraper APIがない？問題ありません。Bright Dataの**AI Scraper Builder**なら、数クリックでカスタムOpenSea scraperを生成できます。コーディングは不要です。

### 数分でOpenSea scraperを構築

**[OpenSea AI Scraper Builderを開く →](https://brightdata.jp/products/web-scraper/opensea)**

ドメインを選択し、必要なデータ要件を記述するだけで、AI scraper builderが自動的にAPIを作成します。

1. **必要なデータを平易な英語で記述**
2. **AIが即座にscraper APIを生成**
3. **APIリクエストを実行してすぐに結果を取得**
4. 必要に応じて**組み込みIDEでコードを編集**

構築が完了すると、scraperには **Web Scraper ID**（`gd_xxxxxxxxxxxx`）が付与されます。以下のSetup手順で使用するため、コピーしておいてください。

### 前提条件

- Python 3.9以上
- [Bright Data account](https://brightdata.jp)（無料トライアルあり）
- Bright Dataの**API token**（[取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)）
- OpenSea用の**Web Scraper ID**（上記の構築手順で取得）

### Setup

1. **このrepositoryをclone**

   ```bash
   git clone https://github.com/bright-jp/opensea-price-tracker.git
   cd opensea-price-tracker
   ```

2. **依存関係をインストール**

   ```bash
   pip install -r requirements.txt
   ```

3. **認証情報を設定**

   `.env.example` を `.env` にコピーし、値を入力します:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **Your Web Scraper ID**
   > [AI Scraper Builder dashboard](https://brightdata.jp/products/web-scraper/opensea) のWeb Scraper IDを
   > `BRIGHTDATA_DATASET_ID` に貼り付けてください（形式: `gd_xxxxxxxxxxxx`）。

---

## Usage

OpenSea scraperの構築が完了し、Web Scraper IDを `.env` に設定すると、Python interfaceは同じ方法で利用できます。

### 1. URLで特定の商品を追跡

OpenSeaの商品URLのリストを渡して、構造化された価格データを取得します:

```python
from price_tracker import track_prices

urls = [
    "https://www.opensea.io/product/sample-item-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

または直接実行:

```bash
python price_tracker.py
```

### 2. キーワードで商品を検索

キーワード検索に一致する商品を見つけます:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. カテゴリURLで商品を閲覧

OpenSeaのカテゴリページからすべての商品を収集します:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://opensea.io/category/example",
    limit=100,
)
```

---

## 出力フィールド

各結果レコードには次のフィールドが含まれます:

| Field | Description |
|-------|-------------|
| `url` | 商品ページURL |
| `title` | 商品名 / タイトル |
| `brand` | ブランドまたはメーカー |
| `initial_price` | 元の価格 / 定価 |
| `final_price` | 現在の販売価格 |
| `currency` | 通貨コード（例: USD、EUR） |
| `discount` | 割引額または割引率 |
| `in_stock` | 商品が利用可能かどうか |
| `rating` | 平均星評価 |
| `reviews_count` | レビュー総数 |
| `seller_name` | 販売者名 |
| `images` | 商品画像URLの配列 |
| `description` | 商品説明テキスト |
| `timestamp` | データ収集タイムスタンプ |

### サンプル出力

```json
[
  {
    "url": "https://www.opensea.io/product/sample-item-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://opensea.io/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高度なオプション

`trigger_collection()` 関数は、データ収集を制御するためのオプションパラメータを受け付けます:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返されるレコードの最大数 |
| `include_errors` | boolean | `true` | 結果にエラーレポートを含める |
| `notify` | string (URL) | - | スナップショットの準備完了時に呼び出されるWebhook URL |
| `format` | string | `json` | 出力形式: `json`、`csv`、または `ndjson` |

オプション付きの例:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.opensea.io/product/sample-item-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## リソース

- 🌟 [OpenSea Price Tracker - Bright Insights (Managed)](https://brightdata.jp/products/insights/price-tracker/opensea)
- 🏗️ [OpenSea Scraperを構築](https://brightdata.jp/products/web-scraper/opensea)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.jp/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.jp/cp/scrapers)
- 🔑 [API tokenの取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://brightdata.jp)

---

*[Bright Data](https://brightdata.jp)で構築 - 業界をリードするWebデータプラットフォーム。*