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


