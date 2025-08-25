# Struts（Apache Struts） 
JavaでWebアプリケーションを作るための オープンソースのWebアプリケーションフレームワーク  
特に 2000年代前半〜中頃に企業システムでよく使われていました  

# Strutsの特徴
MVCアーキテクチャ  
 - M（Model）: ビジネスロジックやデータ（JavaBeans、EJBなど）
 - V（View）: 表示（JSP）
 - C（Controller）: リクエスト制御（Actionクラス）
Struts は Controller 部分を中心に提供 して、リクエストの流れを整理してくれます。  

# Actionベースの仕組み
ユーザーからのリクエストは ActionServlet が受け取る  
struts-config.xml に設定されたマッピングに基づいて、Actionクラス に処理を振り分ける  
Actionクラス が処理した結果を、ActionForward を使って JSP などにフォワードする  
```
ブラウザ → ActionServlet → Actionクラス → ビジネスロジック → JSP
```
