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
