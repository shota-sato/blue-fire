# RDBとの比較
index：テーブル  
document：レコード  
_id：主キー  
 - インデックスは複数のドキュメントを保持します。  
 - 各ドキュメントにはユニークな _id が付きます。  

# _idの上書き保存
```
POST /index-name/_doc/{id}
```
この形式でリクエストを送ると：
id が存在しない → 新規作成
id が存在する → 上書き保存（オーバーライド）

例 1回目投げてみる
```
POST stat_test/_doc/id-2025-08-25T01:08:25.000Z
{
  "time": "2025-08-25T01:08:25.000Z",
  "unit": "id",
  "values": {
    "total": 100,
    "success": 90,
    "failure": 10
  }
}
```
返却値
```
{
  "_index": "stat_test",
  "_id": "id-2025-08-25T01:08:25.000Z",
  "_version": 1,
  "result": "created",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 0,
  "_primary_term": 1
}
```
"_index": "stat_test" なのでData view作成
<img width="1426" height="322" alt="image" src="https://github.com/user-attachments/assets/a26f1965-ab4f-400d-9e04-09b9226b689c" />  
<img width="801" height="397" alt="image" src="https://github.com/user-attachments/assets/cde25eb3-651a-43fa-ae1c-a2728c5b48f0" />  
2回目  
```
POST stat_test/_doc/ds.aset.tps-2025-08-25T01:08:25.000Z
{
  "time": "2025-08-25T01:08:25.000Z",
  "unit": "ds.aset.tps",
  "environment": "production",
  "values": {
    "total": 200,
    "success": 180,
    "failure": 20
  }
}
```
返却値
```
{
  "_index": "stat_test",
  "_id": "ds.aset.tps-2025-08-25T01:08:25.000Z",
  "_version": 2,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 1,
  "_primary_term": 1
}
```
<img width="786" height="380" alt="image" src="https://github.com/user-attachments/assets/eaf1bab2-4bfd-43f1-a8be-1121d4b8603d" />  
上書きされている  

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

# DSL
Domain Specific Language（ドメイン固有言語） の略  
Elasticsearch でいう「DSL」は**Query DSL（クエリ DSL）**  

gte	= greater than = 以上（≧）  
```
"gte": "2025-08-22T18:06:37.527+09:00"
```
lt = less than = 未満（<）
```
"lt": "2025-08-22T18:16:37.527+09:00"
```
"range"：範囲条件で検索せよ
```
{ "range": { "insert_date": { "gte": "2025-08-22T18:06:37.527+09:00" } } },
{ "range": { "insert_date": { "lt":  "2025-08-22T18:16:37.527+09:00" } } }
```
## bool クエリとは？
複数のクエリ条件を論理演算（AND、OR、NOT）を使って組み合わせるための構造。  
主に以下の4つの要素を組み合わせる形で使います  
 - must：必ず満たすべき条件（AND）  
 - filter：must と同じくANDですが、関連性スコアには影響せずキャッシュ対象（スコア不要な場合におすすめ）  
 - should：いずれか満たせばよい条件（OR。ただし minimum_should_match によって制御可能）  
 - must_not：満たしてはいけない条件（NOT）  
**must は AND に相当し、should は OR に相当する**  
公式ドキュメント  
https://www.elastic.co/guide/en/elasticsearch/reference/8.19/query-dsl-bool-query.html?utm_source=chatgpt.com
```
        { "bool": { "must": [
            { "range": { "insert_date": { "gte": "2025-08-22T18:06:37.527+09:00" } } },
            { "range": { "insert_date": { "lt":  "2025-08-22T18:16:37.527+09:00" } } }
        ]}},
```
## query
検索条件のトップレベル定義  
中に bool や range や match などを組み合わせて 対象ドキュメントを絞り込む  
集計 (aggs) は query の結果セットに対して行われる  
```
  "query": {                        // ← 抽出条件（10分間 + 必須フィールド）
    "bool": {
      "must": [
        { "bool": { "must": [
            { "range": { "insert_date": { "gte": "2025-08-22T18:06:37.527+09:00" } } },
            { "range": { "insert_date": { "lt":  "2025-08-22T18:16:37.527+09:00" } } }
        ]}},
        { "exists": { "field": "coupler_sequence" } }
      ]
    }
  },
```
## size: 0 の意味
Elasticsearch の _search API には 検索結果（ヒット）と集計結果（aggregations）の両方を返す仕組みがあります。  
size は「何件の 検索結果（ドキュメント本体） を返すか」を指定するパラメータ。  
size: 0 にすると、ドキュメント本体は返さず、**集計結果だけ返す**ようになります。
<img width="1028" height="590" alt="image" src="https://github.com/user-attachments/assets/38b129af-dee9-4bcb-83ec-916bc7a2bff8" />




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


