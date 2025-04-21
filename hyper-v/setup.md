# 環境構築

### スタートメニューで選択 再起動
![3f98d98f-0c61-474b-bd80-074abed7ba23](https://github.com/user-attachments/assets/7ccb1fdc-8839-459f-a801-bd55762dcf6e)

### クイック作成
![クイック](https://github.com/user-attachments/assets/5d5d13c8-725d-4219-94b7-9e96ca700f59)


![ubuntu](https://github.com/user-attachments/assets/8e4a4d2e-478b-4558-a9e2-509d0f5a1b12)

![access](https://github.com/user-attachments/assets/85a07e70-62bf-40c1-b3a5-8a32bcf3cf85)


![image](https://github.com/user-attachments/assets/0cb5e629-edc6-4e27-bb2a-8763647c8653)

![hello](https://github.com/user-attachments/assets/b6ec8da8-46c7-4e09-8322-688de97b3d45)

### 以降全てskip
![settei](https://github.com/user-attachments/assets/f30b6884-f6bf-4336-8330-89b56ff4fe8d)

### 右クリックで「Open Terminal」
![image](https://github.com/user-attachments/assets/78b7fc02-5c91-4135-be4d-d2853ea335f8)

Hyper-V　のツールを使う上で必要なパッケージのインストール

`sudo apt install linux-virtual linux-cloud-tools-virtual linux-tools-virtual`

`sudo`
管理者権限でコマンドを実行するためのコマンド。システムの設定やソフトウェアのインストールが可能。

`apt`
UbuntuやDebian系のパッケージ管理ツール。
ソフトウェアのインストール、アップデート、削除を行うために使用。

`install`
aptに対するコマンドで、指定したパッケージをインストールすることを指示。

`linux-virtual`
仮想マシン環境で動作するために最適化されたLinuxカーネルのパッケージ。
仮想化環境でのパフォーマンスが向上します。

`linux-cloud-tools-virtual`
クラウド環境での管理や操作に必要なツールを含むパッケージ。
特に、クラウドサービスでの仮想マシンの管理に役立ちます。

`linux-tools-virtual`
仮想マシン環境でのLinuxツールを提供するパッケージ。
パフォーマンスの監視や管理に役立つツールが含まれています。

### viエディタを使用
![image](https://github.com/user-attachments/assets/9e274aa3-88a7-48af-80c4-99460153becc)

grub　linuxのブートローダー
linuxが走り始める前に設定してくれる
`GRUB_CMDLINE_LINUX_DEFAULT_="quiet splash"`

↓
iボタンで編集モードにして「quiet splash」削除


