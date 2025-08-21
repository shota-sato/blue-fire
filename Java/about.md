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
