
# Ingest Pipeline
## dot_expanderプロセッサー
```
{
  "dot_expander": {
    "field": "*",
    "ignore_failure": true
  }
},
```
「ドットを含むフィールド名」を オブジェクトとして展開する役割を持ちます  

例）入力ドキュメント
```
{
  "user.name": "Shota",
  "user.age": 30
}
```
dot_expander 適用後
```
{
  "user": {
    "name": "Shota",
    "age": 30
  }
}
```

# Aggregations
[Aggregations - Elasticsearch 公式ドキュメント](https://www.elastic.co/docs/explore-analyze/query-filter/aggregations?utm_source=chatgpt.com)

どちらのリクエストも 10分間の範囲（now-10min ～ now）を対象にしています。  
1秒ごとのヒストグラム（date_histogram） でバケット分割。  
各秒ごとに "success"（x_result=OK）と "failure"（x_result=NG）をサブ集計。  
size: 0 なのでドキュメント本体は返さず、集計結果だけ を取得  　

データを集約・分析する機能全体のこと。  
例えば「件数を数える」「平均を出す」「期間ごとに分ける」といった処理。  
リクエストの JSON では以下のように書きます
```
"aggregations": { ... }
"aggs": { ... }
```
Elasticsearch は集計を次の3つのカテゴリに分類しています：
 - メトリック集計 (Metric aggregations)  
   フィールド値から合計や平均などのメトリクスを計算する
 - バケット集計 (Bucket aggregations)  
   ドキュメントをフィールドの値、範囲、その他の条件に基づいて「バケット（グループ）」に分ける
 - パイプライン集計 (Pipeline aggregations)  
   ドキュメントやフィールドではなく、他の集計結果を入力として処理を行う

# バケット分割とは？
Elasticsearch の 集計 (aggregation) では、データを「入れ物（バケツ / バケット）」に分けて数えます。  
それを バケット分割 と呼びます。  
イメージは「たくさんのデータを条件ごとに仕分けて、各グループごとに件数や平均を計算する」こと  
（Excel の ピボットテーブル や GROUP BY に近い）  

例：商品ごとの売上件数を知りたい  
```
"aggs": {
  "by_product": {
    "terms": { "field": "product" }
  }
}
```
結果イメージ：
```
"aggregations": {
  "by_product": {
    "buckets": [
      { "key": "りんご", "doc_count": 50 },
      { "key": "バナナ", "doc_count": 30 },
      { "key": "ぶどう", "doc_count": 20 }
    ]
  }
}
```
date_histogram バケット  
今回のログに出ていたのはこちら  
```
"date_histogram": {
  "field": "insert_date",
  "fixed_interval": "1s",
  "min_doc_count": 0
}
```
結果イメージ：  
```
"buckets": [
  { "key_as_string": "2025-08-22T09:16:36Z", "doc_count": 10 },
  { "key_as_string": "2025-08-22T09:16:37Z", "doc_count": 12 },
  { "key_as_string": "2025-08-22T09:16:38Z", "doc_count": 8 }
]
```
各「秒」が 1 バケットになり、その秒に入っているドキュメント数が doc_count に入ります  
さらにサブ集計 "success", "failure" を付けると：  
```
{
  "key_as_string": "2025-08-22T09:16:36Z",
  "doc_count": 10,
  "success": { "doc_count": 7 },
  "failure": { "doc_count": 3 }
}
```

# Dev tools
Kibana の Dev Tools（Console）に投げるときは、**HTTP 行やヘッダーは不要**で、**相対パスだけ**を書きます
curlで実行  
```
curl -X POST "https://エンドポイント/index名-*/_search?search_type=query_then_fetch&typed_keys=true" `
  -H "Authorization: Basic 「userid:passwordをbase64でエンコードした値」" `
  -H "Content-Type: application/json" `
  -d "@query.json"
```

vscodeでのリクエスト  
```
POST https://エンドポイント/index名-*/_search?search_type=query_then_fetch&typed_keys=true
Authorization: Basic 「userid:passwordをbase64でエンコードした値」
Content-Type: application/json

{

}
```
Dev toolsで実行  
```
POST index名-*/_search?typed_keys=true&search_type=query_then_fetch&typed_keys=true
{

}

```

```
search_type=query_then_fetch
```
通常の検索方式（query で候補 → fetch で結果取得）。デフォルト動作。  
明示しなくてもデフォルトなので省略可。
全 shard に問い合わせてスコア付きで候補を集める  
上位 N 件を決めてから、実データを取りにいく  
一般的な検索で最もよく使われ、特別な理由がない限りこれでOKです。
```
typed_keys=true
```
レスポンス JSON のキーに型情報を付与して、パース時の曖昧さをなくす。  
集計やサジェストを使うなら便利、それ以外はなくてもOK。
```
"aggregations": {
  "avg#myagg": {
    "value": 123
  }
}
```
typed_keys=false (デフォルト)  
```
"aggregations": {
  "myagg": {
    "value": 123
  }
}
```


