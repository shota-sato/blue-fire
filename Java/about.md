クラス

フィールド（変数）とメソッド（処理）**を持つことができ、オブジェクト（インスタンス）を作れます
public class Animal {
    public void speak() {
        System.out.println("Some sound");
    }
}

他には
interface（契約だけ）
abstract class（部分的に実装を持つ抽象クラス）
enum（定数の集合）
record（データ用のクラス）



メソッド

Javaのメソッドには必ず戻り値の型を宣言する必要があります

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

戻り値と return の関係
	•	戻り値の型がある場合 → return でその型の値を返さなければならない
	•	void の場合 → return は不要（書いてもいいが値は返せない）


# 抽象クラス (abstract class)
抽象クラスは インスタンスを直接生成できません  
共通の性質や基本的な振る舞いを定義しつつ、一部はサブクラスで必ず実装させるようにできます  
抽象クラスには 抽象メソッド が含まれることがあります。  
## 抽象メソッド
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


## interface との違い
interface   
 - すべてが「抽象メソッド」や「定数」の集合（Java 8以降はdefault/staticメソッドも可）  
abstract class  
 - 「具象メソッド」も「フィールド」も持てるので、部分的に実装を共通化したいときに便利。

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

とりあえずコンパイル
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

プリミティブ型 (primitive type)
Javaに組み込まれている「基本データ型」で、軽くて高速。
値そのものを直接扱います

ラッパー型 (wrapper class)
プリミティブ型を オブジェクトとして扱うためのクラス。
java.lang パッケージに用意されています。

なぜラッパー型が必要？
	1.	コレクションに入れるため
Javaのコレクション（ArrayListなど）はオブジェクトしか扱えません。
だから int ではなく Integer を使う必要があります。
List<Integer> list = new ArrayList<>();
list.add(10); // OK（オートボクシング）

nullを扱える
プリミティブ型には null を入れられませんが、ラッパー型はオブジェクトなので null 可能

Integer i = null;   // OK
int j = null;       // コンパイルエラー

ユーティリティメソッドが使える
例えば Integer.parseInt("123") のように、文字列を数値に変換できる。
int n = Integer.parseInt("123");  // 文字列→int




