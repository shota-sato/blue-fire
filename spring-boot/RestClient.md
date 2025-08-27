
# RestClientとは？
Spring Framework 6 / Spring Boot 3 から導入された
新しい HTTP クライアント（REST API を呼ぶための道具） 

HTTP通信 ＝ 道（プロトコル）
REST API ＝ その道を使ったルール付きのやりとり

HTTP通信とは？
	•	インターネットでやりとりするための基本の通信ルール（プロトコル）
	•	「URLにアクセスする」「リクエスト送る」「レスポンス返す」という枠組み

REST APIとは？
	•	HTTP通信を使った “Webサービス呼び出しの設計スタイル”
	•	単なる通信じゃなくて、「データをどうやって操作するか」をルール化 したもの
	•	特徴：
	•	URL が「資源（リソース）」を表す
	•	例: /users/1 → 「ユーザーID=1の情報」
	•	HTTPメソッドが操作を表す
	•	GET /users/1 → ユーザー取得
	•	POST /users → 新規登録
	•	PUT /users/1 → 更新
	•	DELETE /users/1 → 削除

# Webサービスとは
インターネット（Web）を通じて 機能やデータを提供する仕組み のこと
提供先は 人間のブラウザ画面 だけじゃなく、プログラム同士 でやりとりすることも多い
	•	「画面付き」＝人間がブラウザで使う
	•	「API」＝プログラム同士がやりとりする
	•	RestClient が呼ぶのは後者（API型Webサービス）です


# Spring RESTクライアントの歴史
RestTemplate
	•	古くからあるクラス（Spring 3.x 時代から）
	•	getForObject(), postForEntity() で同期的にHTTP呼び出し
	•	でも今は「古い（非推奨方向）」
WebClient
	•	Spring 5 (WebFlux) で導入
	•	非同期・リアクティブ対応
	•	builder() を使って柔軟に設定できる
RestClient ← 🆕
	•	Spring Framework 6 / Spring Boot 3 (2022年〜) で追加
	•	「WebClient ほど複雑じゃなく、RestTemplate よりモダン」な新しい REST クライアント
	•	同期も非同期もOK
	•	builder() も使える

## RestClient クラス

public final class RestClient {
    
    // これが builder() メソッド
    public static RestClient.Builder builder() {
        return new RestClient.Builder();
    }

    // 内部で使う Builder クラス
    public static class Builder {
        // baseUrl, ヘッダ などの設定メソッドが用意されている
        public Builder baseUrl(String baseUrl) { ... }
        public Builder defaultHeader(String name, String value) { ... }
        public RestClient build() { ... }
    }
}



## RestClient の使い方
RestClient.builder() と書けるのはRestClient クラスの中で 
public static Builder builder() を定義しているから


import org.springframework.web.client.RestClient;

public class Example {
    public static void main(String[] args) {
        RestClient client = RestClient.builder()
            .baseUrl("https://api.example.com")
            .defaultHeader("Authorization", "Bearer token")
            .build();

        String result = client.get()
            .uri("/users/1")
            .retrieve()
            .body(String.class);

        System.out.println(result);
    }
}

## RestClient.builder()
	•	RestClient.builder()
→ 「ビルダーを呼び出す」＝設定用のスタート地点
	•	.baseUrl("https://api.example.com")
→ 「このクライアントは https://api.example.com を基本のURLにするよ」
	•	.defaultHeader("Authorization", "Bearer token")
→ 「毎回リクエストに Authorization ヘッダをつけて送るよ」
	•	.build()
→ ここで初めて 完成品の RestClient ができあがる

→ 「ビルダーを呼び出す」＝設定用のスタート地点　　
	•	client.get()
→ 「GET メソッドでリクエストを作るよ」
	•	.uri("/users/1")
→ 「https://api.example.com/users/1 にアクセスするよ」
（baseUrl と合体する）
	•	.retrieve()
→ 「リクエストを送ってレスポンスを受け取るよ」
	•	.body(String.class)
→ 「レスポンスの中身を文字列として読み込むよ」


# Builder パターン 
	•	Javaの言語仕様や標準ライブラリが用意しているわけじゃない
	•	もともと プログラミング界で広く共有されている設計の知恵
Spring はフレームワーク内で.builder() を最初から持っている




