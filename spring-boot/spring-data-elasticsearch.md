# アノテーション一覧
```
@Document(indexName = "fep_buyinfo_request-*")
public class FepBuyInfoEntity { ... }
```
このクラスを Elasticsearch のドキュメント（レコード）と対応付けるためのマーカー。
RDB の JPA で言う @Entity に近いもの



# ElasticsearchOperations
```
public interface ElasticsearchOperations extends DocumentOperations, SearchOperations, ScriptOperations { ... }
```
 - パッケージ: org.springframework.data.elasticsearch.core  
 - 提供元: Spring Data Elasticsearch  
 - 種別: interface（インターフェース） 

Elasticsearch操作用の統合インターフェース  
ドキュメント操作（CRUD） → DocumentOperations 継承  
検索（query, aggregation, scroll など） → SearchOperations 継承  
スクリプト操作 → ScriptOperations 継承  
Index操作 → indexOps(...)  
Cluster操作 → cluster()  
Converter取得 → getElasticsearchConverter()  
Routing指定 → withRouting(...)  
RefreshPolicy設定 → withRefreshPolicy(...)  

# Criteria 
Spring Data Elasticsearch が提供している 検索条件を表すオブジェクト   
イメージとしては、SQL の WHERE 句 を Java のオブジェクトで書けるようにしたもの   
それを CriteriaQuery に渡すと、最終的に Elasticsearch のクエリ DSL（JSON）に変換されます  

SQLの場合  
```
SELECT * 
FROM sample_t
WHERE insert_date >= '2025-08-19T00:00:00'
  AND insert_date <  '2025-08-19T01:00:00'
  AND a_sequence IS NOT NULL;  
```
Spring Data Elasticsearch の Criteria で書くと  
```
Criteria insertDateIsBetweenFromAndTo = Criteria
        .where("insert_date")
        .greaterThanEqual(JsonData.of(from.format(FORMATTER)))
        .lessThan(JsonData.of(to.format(FORMATTER)));

Criteria aSequenceExists = Criteria
        .where("a_sequence")
        .exists();

CriteriaQuery query = new CriteriaQuery(
        insertDateIsBetweenFromAndTo.and(aSequenceExists)
);
```

```
.greaterThanEqual(JsonData.of(from.format(FORMATTER)))
```
「この値以上 (>=)」という条件を追加  
from は ZonedDateTime 型なので、文字列（ISO形式 "yyyy-MM-dd'T'HH:mm:ss.SSSXXX"）にフォーマットして JSON 値に変換  
例: 2025-08-19T10:00:00.000+09:00  
```
.lessThan(JsonData.of(to.format(FORMATTER)))
```
「この値未満 (<)」という条件を追加  
to も同様に文字列に変換して使う  


# NativeQuery
```
NativeQuery nativeQuery = new NativeQueryBuilder()
        .withQuery(QueryBuilders.match("title", "Java"))
        .build();
```
Javaのオブジェクト指向的な「ビルダーパターン」を使って **クエリ（検索条件オブジェクト）を組み立てている**  
**「Elasticsearch の Java 用クライアント（Spring Data Elasticsearch など）」でよく使われる書き方。**   
SQLの文字列を書く代わりに、Javaの型安全なオブジェクトとしてクエリを組み立てられるのがメリット  
 - NativeQuery ： **クエリを表すクラス**
 - NativeQueryBuilder ： **NativeQuery を組み立てるためのビルダー（補助クラス）**
 - .withQuery(...) ：**検索条件を設定**
 - .build() ：**NativeQuery インスタンスを生成**
   
.withAggregation("completion_rate", …)
意味：ビルダーに 1個の集計（aggregation） を追加します。
第1引数 "completion_rate"：その集計に付ける名前（キー）。
レスポンスでも同じキーで返ってくるので、結果を取り出すときに使います。
```
{
  "aggs": {
    "completion_rate": { …ここに集計の本体… }
  }
}
```

