# クラス  
フィールド（変数）とメソッド（処理）を持つことができ、オブジェクト（インスタンス）を作れます
```
public class Animal {
    public void speak() {
        System.out.println("Some sound");
    }
}
```
他には  
 - interface（契約だけ）  
 - abstract class（部分的に実装を持つ抽象クラス）  
 - enum（定数の集合）  
 - record（データ用のクラス）  

# メソッド
Javaのメソッドには必ず**戻り値の型を宣言**する必要があります
```
// int型を返すメソッド
public int add(int a, int b) {
    return a + b;  // returnで必ずintを返す
}

// String型を返すメソッド
public String greet(String name) {
    return "Hello, " + name;
}

// 戻り値なし
public void printHello() {
    System.out.println("Hello!");
}
```

# 戻り値と return の関係
戻り値は**メソッド内**に記載
 - 戻り値の型がある場合 → return でその型の値を返さなければならない
 - void の場合 → return は不要（書いてもいいが値は返せない）

# 抽象クラス (abstract class)
抽象クラスは **インスタンスを直接生成できません**  
共通の性質や基本的な振る舞いを定義しつつ、一部はサブクラスで必ず実装させるようにできます  
抽象クラスには **抽象メソッドが含まれることがあります**。  

# 抽象メソッド
「名前と引数は決まっているけど、中身（処理内容）がないメソッド」  
「抽象クラスはインスタンスを直接生成できない理由」は、抽象クラスは「不完全な設計図」だからです  
```
abstract class Animal {
    // 抽象メソッド（処理内容は未定義）
    abstract void makeSound();
    // 具象メソッド（通常通り処理を持つ）
    void breathe() {
        System.out.println("息をしています");
    }
}
```
```
class Dog extends Animal {
    // 抽象メソッドを必ず実装
    @Override
    void makeSound() {
        System.out.println("ワンワン！");
    }
}
class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("ニャー！");
    }
}
```

## 単一継承
```
   Parent
   ---------
   + methodP()

       ▲
       |
   Child
   ---------
   + methodC()
```
クラス同士は 1つだけ継承 (extends) が可能  

## クラスの多重継承NG
```
   ParentA        ParentB
   ---------      ---------
   + methodA()    + methodB()

       ▲     ▲
        \   /
         \ /
        Child
   -----------------
   + methodA()
   + methodB()
```
Java では このように複数のクラスを同時に継承することはできません  



# インタフェース
**「契約（仕様書）」** だけを書くもの  
「このメソッドを必ず持ってください」という宣言  
メソッドの名前・引数・戻り値は定義するが、中身は書かない  
インターフェースは**クラスの一部でも逆でもなく、別の仕組み**  
**インスタンス化は抽象クラスと同じで出来ない**    

## クラスがインターフェースを「実装」するとは
```
interface Animal {
    void eat();   // 食べるメソッド
    void sleep(); // 寝るメソッド
}

class Dog implements Animal {
    @Override
    public void eat() {
        System.out.println("ドッグフードを食べる");
    }

    @Override
    public void sleep() {
        System.out.println("丸まって寝る");
    }

    // クラス独自のメソッドも書ける
    public void bark() {
        System.out.println("ワンワン！");
    }
}
```

## ポリモーフィズム（多態性）とは
同じインターフェースや親クラス型で扱っても、実際の中身によって振る舞いが変わること  　　
**インターフェースにメソッドが2個あれば、必ず2個以上は実装しなきゃいけない。**  
でも クラスは自由にメソッドを増やせる。 
例1  インタフェースで実装
```
interface Animal {
    void eat();
    void sleep();
}

class Dog implements Animal {
    @Override
    public void eat() {
        System.out.println("ドッグフードを食べる");
    }

    @Override
    public void sleep() {
        System.out.println("丸まって寝る");
    }

    // クラス独自のメソッド
    public void bark() {
        System.out.println("ワンワン！");
    }
}
```
例2 クラスを使って継承する
```
class Animal {
    void speak() {
        System.out.println("???");
    }
}

class Dog extends Animal {
    @Override
    void speak() {
        System.out.println("ワンワン！");
    }
}

class Cat extends Animal {
    @Override
    void speak() {
        System.out.println("ニャー！");
    }
}
```



## interface との違いと使い分け
interface  
 - すべてが「抽象メソッド」や「定数」の集合（Java 8以降はdefault/staticメソッドも可）  
abstract class  
 - 「具象メソッド」も「フィールド」も持てるので、部分的に実装を共通化したいときに便利。
  
抽象クラス = 「グループの共通部分をまとめる」  　
インタフェース = 「機能の契約を定める」  

# スーパークラス
```
class Animal {
    void makeSound() {
        System.out.println("Some sound...");
    }
}
```
```
class Dog extends Animal {
    void makeSound() {
        super.makeSound();   // ← 親クラス Animal の makeSound() を呼ぶ
        System.out.println("Woof!");
    }
}
```

# とりあえずコンパイル
```
public abstract class RecruitCompletionRateRepositoryImpl implements CompletionRateRepository {

  @Override
  public CompletionRate calculateCompletionRate(ZonedDateTime from, ZonedDateTime to) {

    return new CompletionRate(null,null,null,null,null,null,null);
}

public record CompletionRate(
        ZonedDateTime time,
        Long total,
        Long success,
        Long failure,
        String channelCd,
        String payMethod,
        Duration durationMinutes) {

    public Double successRate() {
        //total が null なら 0L、そうでなければ total自身
        Long totalInternal = (total == null) ? 0L : total;
        Long successInternal = (success == null) ? 0L : success;
        Long failureInternal = (failure == null) ? 0L : failure;

        //total=0 の場合、successrateは0.0
        return (totalInternal <= 0) ? 0.0 : (double) successInternal / totalInternal;
    }
}

```

# プリミティブ型 (primitive type)
Javaに組み込まれている「基本データ型」で、軽くて高速  
値そのものを直接扱います  

# ラッパー型 (wrapper class)
プリミティブ型を オブジェクトとして扱うためのクラス  
java.lang パッケージに用意されています  

## なぜラッパー型が必要？
 - コレクションに入れるため
   Javaのコレクション（ArrayListなど）はオブジェクトしか扱えません。
   だから int ではなく Integer を使う必要があります。
```
List<Integer> list = new ArrayList<>();
list.add(10); // OK（オートボクシング）
```
 - nullを扱える
   プリミティブ型には null を入れられませんが、ラッパー型はオブジェクトなので null 可能
```
Integer i = null;   // OK
int j = null;       // コンパイルエラー
```
 - ユーティリティメソッドが使える
   例えば Integer.parseInt("123") のように、文字列を数値に変換できる。
```
int n = Integer.parseInt("123");  // 文字列→int
```

# @Override の役割
オーバーライドしていることを明示する  
これは親クラスのメソッドを上書きしている  
シグネチャ（メソッド名や引数の型）が間違っていた場合、コンパイルエラーになる。  

# record
**Java 16 で正式導入**された「データキャリア専用のクラス定義方法」  
「**値を保持するだけのクラス**を、超簡潔に書ける構文」  
従来の書き方  
```
public class CompletionRate {
    private final ZonedDateTime time;
    private final Long total;
    private final Long success;
    private final Long failure;

    // コンストラクタ
    public CompletionRate(ZonedDateTime time, Long total, Long success, Long failure) {
        this.time = time;
        this.total = total;
        this.success = success;
        this.failure = failure;
    }

    // getter
    public ZonedDateTime getTime() { return time; }
    public Long getTotal() { return total; }
    public Long getSuccess() { return success; }
    public Long getFailure() { return failure; }

    // equals / hashCode / toString も書く…
}
```
record を使うと同じことが 1行で定義  
```
public record CompletionRate(
    ZonedDateTime time,
    Long total,
    Long success,
    Long failure,
    String channelCd,
    String payMethod,
    Duration durationMinutes) { }
```

## private 
外から直接アクセスできない（カプセル化）
## final
一度値を入れたら変えられない（不変オブジェクトになる）

継承はクラスだけでなくインターフェース同士でもできます
クラス → クラスを extends できる（ただし単一継承のみ）
インターフェース → インターフェースを extends できる（複数継承OK）

extends → 「親の設計図を引き継ぐ」
implements → 「仕様書に従って作る」
インターフェース = 契約書（「このメソッドを持つこと！」だけ書いてある）
implements = 契約を守って、実際の中身を書くこと


Dration は Java 8 以降の java.time パッケージにある、
“時間量（経過時間）” を表すクラスです。秒とナノ秒の精度で、不変（immutable）


# Mapインターフェース
型引数が2つ必要  
キーと値のペア（Key-Value） を扱うコレクション    
 - 配列やリストは「順番で要素を管理」しますが、Mapは「キーで要素を管理」します。
 - 同じキーは一度しか保持できません（キーは一意）。
 - 値は重複してもOK
Map のキーや値に プリミティブ型は直接使えない ので、代わりに**ラッパー型 を使う必要があります**    
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Map.html  
```
Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 90);
scores.put("Bob", 75);
scores.put("Charlie", 85);

System.out.println(scores.get("Alice")); // 90
```

# オブジェクトとは
クラスから作られた実体（実際に動くもの） のこと  
クラスは「設計図」、オブジェクトは「その設計図から作られた製品」とよく例えられます。

# ofメソッド
Java 9+ で追加された不変コレクション生成のための便利メソッド  
of メソッドは static メソッドとして以下に用意されています
 - List.of(...)  
 - Set.of(...)   
 - Map.of(...)  
つまり「インスタンス化用のショートカット」として使えます  
```
List<String> fruits = List.of("apple", "banana", "cherry");
System.out.println(fruits); // [apple, banana, cherry]

Set<Integer> numbers = Set.of(1, 2, 3, 4, 5);
System.out.println(numbers); // [1, 2, 3, 4, 5]

Map<String, Integer> ages = Map.of(
    "Alice", 20,
    "Bob", 25,
    "Charlie", 30
);
System.out.println(ages); // {Alice=20, Bob=25, Charlie=30}
```

# String 型ってなに？  
String は Java に用意されている 文字列を扱うクラス  
"Hello" のような文字列は String クラスのオブジェクト  
Java には標準でたくさんのクラスがある  

Java で最初から使える String, List, Map, Integer, LocalDate などは  
すべて JDK（Java Development Kit） に入っています。  
具体的には Java 標準ライブラリ（java.base モジュールなど）として提供されています。

# JDKに含まれているクラスのパッケージ
 - java.lang（自動importされる）  
   String, Object, Math, Integer, Double など

 - java.util  
   List, Map, Set, ArrayList, HashMap など

 - java.time  
   LocalDate, LocalTime, ZonedDateTime など

 - java.io / java.nio  
   File, InputStream, BufferedReader など

