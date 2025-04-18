# Hyper-Vとは
Hyper-Vは、Microsoftが提供する仮想化技術です。
物理サーバー上に複数の仮想マシンを作成し、効率的にリソースを活用できます

## 仮想化の仕組み
物理マシンのリソースを効率的に活用するための技術
物理マシン上に仮想レイヤーを作成し、その上で複数の仮想マシンを動作させます

## Hyper-VとVMwareの違い
Hyper-VとVMwareは、両者とも仮想化技術を提供する主要なプラットフォームですが、
いくつかの重要な違いがあります。

Hyper-VはMicrosoft製で、Windows環境との親和性が高く、比較的低コストで導入できます。
一方、VMwareは幅広いプラットフォームに対応し、より豊富な管理機能を持ちます。
パフォーマンスや安定性では、VMwareが若干優位とされています

### 仮想化技術
Hyper-Vは、**ベアメタル型（タイプ1）のハイパーバイザー**を使用し、**ハードウェアに直接インストール**されます。
これにより、オーバーヘッドが少なく、高いパフォーマンスを実現できる

VMwareは、**ホスト型（タイプ2）と、ベアメタル型の両方のハイパーバイザー**を提供しています。
**ホスト型は既存のOSの上で動作し、導入が容易**ですが、パフォーマンスはやや劣ります。
ベアメタル型のESXiは、Hyper-V同様に高いパフォーマンスを発揮します

![image](https://github.com/shota-sato/blue-fire/blob/e6fddb38f3da283d64191c02bc436f7a2fa22644/hyper-v/images/kasouka.PNG)
![image](https://github.com/shota-sato/blue-fire/blob/bb2750c2c5056678350cc9402f604da9f15499d9/hyper-v/images/hypervisor-type.PNG)

