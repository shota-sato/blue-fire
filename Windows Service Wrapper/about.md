Windows Service Wrapper

🔹 Windowsサービスとは？
	•	Windowsが提供する「常駐アプリケーション」の仕組み
	•	OS起動時に自動で立ち上がり、ユーザーがログインしてなくても動き続ける
	•	例：SQL Server、IIS、Print Spooler


🔹 Windowsサービスラッパーとは？
👉 普通のアプリケーションを Windows サービスとして動かせるようにする「包み紙（ラッパー）」 のことです。
	•	通常の Java アプリやバッチプログラムは「exe」じゃないから直接 Windows サービス化できない
	•	そこで「ラッパー」が間に入り、アプリを サービスとして登録・制御（開始/停止/再起動） できるようにする

🔹 よく使われるラッパーツールの例
	1.	Apache Commons Daemon (procrun)
	•	Tomcat が Windows サービスとして動くときに使われている仕組み
	•	任意の Java アプリをサービス化可能
	2.	WinSW (Windows Service Wrapper)
	•	XML設定で簡単にサービス化できるツール
	•	Jenkins を Windows サービス化するのに使われることが多い
	3.	NSSM (Non-Sucking Service Manager)
	•	任意の実行ファイルをサービス化できる便利ツール
	•	Javaに限らず Python や Node.js アプリでも使える

[Windowsサービス管理] ←→ [ラッパー(WinSW/procrun)] ←→ [Javaアプリ]

実際にはラッパーが裏で java -jar myapp.jar を起動してくれている

.exe とは？
	•	拡張子 .exe = executable file の略（実行可能ファイル）
	•	ダブルクリックすると WindowsのOSが直接読み込んで動かせる形式
Windowsで実行できるプログラムファイルの拡張子

Javaアプリは「そのままでは Windows のサービスに登録できない
	•	Windowsのサービス管理（services.msc）に登録できるのは、基本的に .exe（実行可能ファイル） です。
	•	サービスは OS が直接 .exe を起動・停止・監視します。



