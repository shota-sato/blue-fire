
@Entity アノテーション
このクラスが「DBのテーブルとマッピングされる」ことを示します. 

@Id
エンティティには必ず「主キー（ユニークに特定するID）」が必要

Java側（メモリ上でのデータ保持）
Javaはオブジェクト指向の世界
データは オブジェクトのフィールド として保持される
JVM（Java Virtual Machine）のヒープ領域にオブジェクトが作られ、変数で参照される
プログラムを終了すると、このオブジェクトは消えます（揮発さし


データベース側（永続化されたデータ）
データベースは**リレーショナル（表形式）**の世界　データは**テーブルの行（レコード）**として保持される
ディスク（永続ストレージ）に保存されるため、プログラムを終了しても残る（永続性あり）

ORMとは
Object Relational Mapping の略  
オブジェクト（Javaのクラスやインスタンス）と、
リレーショナルDB（表形式のデータ）を対応付ける仕組み のこと　　

JPA (Java Persistence API)   
Javaの標準仕様　　
JPAそのものは「設計図」みたいなもので、実際に動かすのは実装ライブラリです。
（Hibernateが一番よく使われます　　

JPAのメリット
SQLを書かなくてもDB操作できる　　
DBに依存しない（MySQLでもPostgreSQLでも基本同じコードでOK）　　
オブジェクト思考的にプログラムを書ける　　

@RequestMapping("/api/users")
public class UserController {
    private final UserService service;

    public UserController(UserService service) {
        this.service = service;
    }

DI = Dependency Injection（依存性注入） の略。
	•	依存性 = 「このクラスが別のクラスに頼って動いていること」
	•	注入 = 「頼っているクラスを外から渡してもらうこと」
自分で new しないで、Spring に作ってもらったオブジェクトをコンストラクタやフィールドで受け取る


@RestController
@RequestMapping("/api/users")
public class UserController {
    private final UserService service;

    // コンストラクタで受け取る＝DI
    public UserController(UserService service) {
        this.service = service;
    }

    // serviceを使って処理
}

Spring が裏でこういうことをやっています：
	1.	Spring がアプリ起動時に @Service のついた UserService のインスタンスを作成
	2.	Spring が UserController を作るときに、このコンストラクタを見て
「あ、UserService が必要なんだな」
	3.	Spring が管理している UserService のインスタンスをここに**注入（Inject）**してくれる

Springがやっていることの流れ
	1.	アプリ起動時に、Springはクラスパスをスキャン
	•	@Component, @Service, @Repository, @Controller などのアノテーションを探す
	2.	見つけたクラスを インスタンス化（newする）
	•	これが「Beanを作る」って言い方をされる
	•	例：UserService に @Service が付いていれば、Springが new UserService() して管理する
	3.	依存関係がある場合は、コンストラクタやフィールドに 注入（Inject）
	•	例：UserController(UserService service) ってコンストラクタがあったら
Springはすでに作っておいた UserService のインスタンスを引数に渡して new UserController(service) を実行
	4.	出来上がったオブジェクト（Bean）を IoCコンテナ（＝Springの箱の中）で管理する
	•	もうアプリ内で勝手に new しなくても、その箱から取り出して使える


